---
layout: post
title: "[Spring] Spring Transaction"
subtitle: "Spring transaction"
categories: study
tags: springboot
---
> Spring Transaction

## Java Database conntct

자바에서 DB와 Connect를 하는 방식은 다음과 같다.

```java
		String url = "jdbc:mariadb://localhost:3306/user_msa";
		String user = "user";
		String pwd = "user123";

		try(Connection conn = DriverManager.getConnection(url, user, pwd)) {
			Statement stmt = null;
			ResultSet rs = null;

			stmt = conn.createStatement ();
			rs = stmt.executeQuery ( "select count(*) from post" );
			rs.next ();

			int cnt = rs.getInt ( "count(*)" );
			System.out.println ( cnt );

		} catch (SQLException throwables) {
			throwables.printStackTrace ( );
		}

```

DriverManager를 통해 Connection객체를 생성한 후 Statement를 생성하여 쿼리를 작성하고, resultset을 받아오는 방식이다. 이때 생성된 connection과 statement는 꼭 닫아주도록 해야한다.

## business logic VS Data Logic

위 와 같은 방식으로 Connection을 얻고, 쿼리를 날리는 방식을 Service 로직에 작성하게 된다면 비지니스 로직과 데이터 로직이 서로 뒤 섞여 버린다. 이것은 비지니스 로직이라는 단일 책임을 지켜야 하는 서비스 객체로의 설계가 잘못되었음을 뜻한다.

따라서 Data에 Access, Quering, data get 하는 책임을 나눈 다른 객체를 DI받아 비지니스 로직에서 이용하도록 변경을 한다. 

```java
public class Service{
	private final JdbcTemplate jdbcTemplate;

	public Service(DataSource datasource){
		this.jdbcTemplate = new JdbcTemplate(datasource);
	}

	public void calcCount(){

		...
		logic
		...
	}

}
```

JdbcTemplate는 멤버변수로 DataSource를 갖고있는 객체이다. JdbcTemplate의 내부 로직을 따라 들어가면 다음과 같은 부분을 볼 수 있다.

```java
public <T> T execute(ConnectionCallback<T> action) throws DataAccessException {
		Assert.notNull(action, "Callback object must not be null");

		Connection con = DataSourceUtils.getConnection(obtainDataSource());
		try {
			// Create close-suppressing Connection proxy, also preparing returned Statements.
			Connection conToUse = createConnectionProxy(con);
			return action.doInConnection(conToUse);
		}
		catch (SQLException ex) {
			// Release Connection early, to avoid potential connection pool deadlock
			// in the case when the exception translator hasn't been initialized yet.
			String sql = getSql(action);
			DataSourceUtils.releaseConnection(con, getDataSource());
			con = null;
			throw translateException("ConnectionCallback", sql, ex);
		}
		finally {
			DataSourceUtils.releaseConnection(con, getDataSource());
		}
	}
```

위 코드를 보면 Connection con = DataSourceUtils.getConnection(obtainDataSource()); 를 확인할 수 있는데 여기에서 DataSourceUtils의 내부에서 connection을 받아오는 부분을 확인하면 내부에서 

```java
public static Connection doGetConnection(DataSource dataSource) throws SQLException {
		Assert.notNull(dataSource, "No DataSource specified");

		ConnectionHolder conHolder = (ConnectionHolder) TransactionSynchronizationManager.getResource(dataSource);
		if (conHolder != null && (conHolder.hasConnection() || conHolder.isSynchronizedWithTransaction())) {
			conHolder.requested();
			if (!conHolder.hasConnection()) {
				logger.debug("Fetching resumed JDBC Connection from DataSource");
				conHolder.setConnection(fetchConnection(dataSource));
			}
			return conHolder.getConnection();
		}
		// Else we either got no holder or an empty thread-bound holder here.

		logger.debug("Fetching JDBC Connection from DataSource");
		Connection con = fetchConnection(dataSource);

		if (TransactionSynchronizationManager.isSynchronizationActive()) {
			try {
				// Use same Connection for further JDBC actions within the transaction.
				// Thread-bound object will get removed by synchronization at transaction completion.
				ConnectionHolder holderToUse = conHolder;
				if (holderToUse == null) {
					holderToUse = new ConnectionHolder(con);
				}
				else {
					holderToUse.setConnection(con);
				}
				holderToUse.requested();
				TransactionSynchronizationManager.registerSynchronization(
						new ConnectionSynchronization(holderToUse, dataSource));
				holderToUse.setSynchronizedWithTransaction(true);
				if (holderToUse != conHolder) {
					TransactionSynchronizationManager.bindResource(dataSource, holderToUse);
				}
			}
			catch (RuntimeException ex) {
				// Unexpected exception from external delegation call -> close Connection and rethrow.
				releaseConnection(con, dataSource);
				throw ex;
			}
		}

		return con;
	}

	private static Connection fetchConnection(DataSource dataSource) throws SQLException {
			Connection con = dataSource.getConnection();
			if (con == null) {
				throw new IllegalStateException("DataSource returned null from getConnection(): " + dataSource);
			}
			return con;
	}
```

이렇게 connection을 받아오는 모습을 볼 수 있다. 여기서 이렇게 복잡한 과정을 거쳐 connection을 받아오는 이유는 스프링의 트랜잭션 동기화(transaction synchronization)을 이용하기 위해서다.  

`트랜잭션 동기화 ( Transaction Synchronization )` : 트랜잭션을 시작하기 위한 connection을 일정한 저장소에 보관해 두고 connection을 요구할때 저장소에 저장된 커넥션을 이용하도록 하는 방식  

만약 매 트랜잭션마다 datasource  → connection → statement → resultset 이러한 방식을 이용한다면 매번 새로운 connection을 열어 DB와 연결을 해야하는 비효율적 방식임을 알 수 있다.  

따라서 트랜잭션 동기화를 통해 이러한 비효율을 줄이도록 하는것이다.

```java
public class Service{
	private DataSource datasource;

	public Service(DataSource datasource){
		this.datasource = datasource;
	}

	public void quering(){
		TransactionSynchronizationManager.initSynchromization();
		Connection conn = DataSourceUtils.getConnection(this.datasource);
		conn.setAutoCommit(false);

		try{

		} catch (Exception e) {
				conn.rollback();
		} finally{
			DataSourceUtils.releaseConnection(conn, this.datasource);
			TransactionSynchronizationManager.unbindResource(this.datasource);
			TransactionSynchronizationManager.clearSynchronization();
		}

	}

}
```

`TransactionSynchronizationManager`를 사용하게 되면 그때 그때 connection 객체를 생성하는것이 아닌 connection 저장소에서 커넥션을 가져와서 이용할 수 있도록 해준다. 또한 이러한 저장소는 스레드마다 별도로 저장되므로 멀티 스레드 환경으로 돌아가는 스프링 프레임워크와도 잘 들어맞는다.

이러한 방식은 트랜잭션 동기화를 통해 매번 connection객체를 받지 않아도 되는 장점이 있지만, 위 매서드의 경우 한번의 메서드 호출에 commit/rollback이 한번씩 일어나는 로컬 트랜잭션 방식이다. 따라서 이러한 방식을 탈피해 글로벌 트랜잭션 방식을 이용해야 한다.

JDBC에선 이러한 해결책으로 JTA ( Java Transaction API )를 제공하지만, Hibernate의 경우 connection이 아닌 session을 이용하게 되며 플랫폼에 의존적이지 않도록 추상화된 트랜잭션이 필요하게 되었다.

이 부분을 위해 추상화된 객체 PlatformTransactionManager를 사용하게 되었다.

## 트랜잭션 속성

1. 전파 ( Transaction Propagation )
    1. PROPAGATION_REQUIRED ( default )

        진행중인 트랜잭션이 없으면 새로 시작하고, 있으면 그 트랜잭션에 참여한다.

    2. PROPAGATION_REQUIRES_NEW

        항상 새로운 트랜잭션을 시작한다. 이미 진행중인 트랜잭션이 있으면 보류 시킨다. 

    3. PROPAGATION_NOT_SUPPORTED

        트랜잭션이 없도록 동작하도록 할 때 사용한다. 이미 트랜잭션이 있으면 보류시킨다.

    4. PROPAGATION_SUPPORTS

        이미 시작된 트랜잭션이 있으면 참여하고, 없으면 트랜잭션 없이 진행

    5. PROPAGATION_MANDATORY

        이미 트랜잭션이 있으면 참여한다. 만약 없으면 예외를 발생시킨다.

    6. PROPAGATION_NEVER

        트랜잭션을 이용하지 않도록 강제한다.. 이미 트랜잭션이 있으면 예외를 발생시킨다.

    7. PROPAGATION_NESTED ( JpaTransactionManager에서 지원안함 )

        이미 트랜잭션이 있으면 중첩 트랜잭션을 발생시킨다. 중첩 트랜잭션은 부모 트랜잭션의 커밋, 롤백에는 영향을 받지만 반대의 영향은 주지않는다. 

    ![transaction](/assets/img/SpringBoot/transaction.png)

2. 격리수준 ( Isolation Level )
    1. DEFAULT

        DB드라이버의 디폴트 설정을 따른다. 대부분의 DB들은 READ_COMMITED설정을 하고있다

    2. READ_UNCOMMITED
    가장 낮은 수준의 격리수준. 하나의 트랜잭션이 커밋되기 전에 다른 트랜잭션에 해당 값을 조회하면 변겨된 정보가 표출되는 DIRTY_READ문제가 발생할 수 있다.
    3. READ_COMMITED

        가장 많이 사용되는 격리수준. 하나의 트랜잭션이 커밋을 한 값만 다른 트랜잭션이 조회할때 표출된다. 그러나 한 트랜잭션이 읽은 값은 다른트랜잭션에서 변경할 경우 처음 조회한 값과는 다른값이 될 수 있는 NON_REPEATABLE_READ문제가 발생할 수 있다.

    4. REPEATABLE_READ

        하나의 트래잭션이 조회한 row를 다른 트랜잭션이 변경하는것을 막는 옵션이다. 그러나 다른 트랜잭션에서 row를 추가 하거나 처음 조회한 조건에 부합하도록 다른 row를 변경하게 될 경우 새로 생긴 row가 생겨나는 PHANTOM 문제 발생할 수 있다.

3. 제한시간
4. 읽기전용
5. 롤백 예외
    1. rollbackFor

        transactional의 경우 uncheckedException에 대해서만 rollback을 지원한다. 트랜잭션 경계 안에서 일어나는 chckedException의 경우 정상적인 비지니스 로직이라 보기 때문이다. 그러나 이러한 checkedException중 rollback을 일으키고싶은 경우 rollbackFor 옵션에 해당되는 checkedException클래스를 추가해주면 된다.

6. 커밋 예외

    1. noRollbackFor

        롤백 예외와 완전 반대 개념으로, uncheckedException중에서 발생해도 커밋을 일으키고 싶은 exception 클래스를 추가해주면, 해당 uncheckedException이 발생해도 커밋된다.

## @Transactional 어노테이션

타입과 메소드에 적용할 수 있는 어노테이션이다. 따라서 class, interface, method에 적용이 가능하다. 

## 선언적 트랜잭션

이런식으로 AOP를 이용해 코드의 외부에서 트랜잭션의 기능과 속성을 지정해주는것을 선언적 트랜잭션이라고 한다.

## 트랜잭셔널 어노테이션의 대체정책

```java
[1]
public interface Service{
[2]
	void method1();
}

[3]
public class ServiceImpl implements Service{
[4]
	public void method1(){

	}

}
```

위와 같이 [1]~[4]의 위치에 @Transactional 어노테이션을 붙일 수 있고, 적용되는 우선순위는 [4] → [1] 이다.이러한 대체 정책을 통해 메서드에 따라 세세하게 트랜잭션 경계설정이 가능하다.

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {

	/**
	 * Alias for {@link #transactionManager}.
	 * @see #transactionManager
	 */
	@AliasFor("transactionManager")
	String value() default "";

	@AliasFor("value")
	String transactionManager() default "";

	String[] label() default {};

	Propagation propagation() default Propagation.REQUIRED;

	Isolation isolation() default Isolation.DEFAULT;

	int timeout() default TransactionDefinition.TIMEOUT_DEFAULT;

	String timeoutString() default "";

	boolean readOnly() default false;

	Class<? extends Throwable>[] rollbackFor() default {};

	String[] rollbackForClassName() default {};

	Class<? extends Throwable>[] noRollbackFor() default {};

	String[] noRollbackForClassName() default {};

}
```

Propagation Enum

```java
public enum Propagation {
	REQUIRED(TransactionDefinition.PROPAGATION_REQUIRED),
	SUPPORTS(TransactionDefinition.PROPAGATION_SUPPORTS),
	MANDATORY(TransactionDefinition.PROPAGATION_MANDATORY),
	REQUIRES_NEW(TransactionDefinition.PROPAGATION_REQUIRES_NEW),
	NOT_SUPPORTED(TransactionDefinition.PROPAGATION_NOT_SUPPORTED),
	NEVER(TransactionDefinition.PROPAGATION_NEVER),
	NESTED(TransactionDefinition.PROPAGATION_NESTED);

	private final int value;

	Propagation(int value) {
		this.value = value;
	}

	public int value() {
		return this.value;
	}

}
```

Isolation Enum

```java

public enum Isolation {

	DEFAULT(TransactionDefinition.ISOLATION_DEFAULT),
	READ_UNCOMMITTED(TransactionDefinition.ISOLATION_READ_UNCOMMITTED),
	READ_COMMITTED(TransactionDefinition.ISOLATION_READ_COMMITTED),
	REPEATABLE_READ(TransactionDefinition.ISOLATION_REPEATABLE_READ),
	SERIALIZABLE(TransactionDefinition.ISOLATION_SERIALIZABLE);

	private final int value;

	Isolation(int value) {
		this.value = value;
	}

	public int value() {
		return this.value;
	}

}
```

## 스프링의 트랜잭션 경계설정 어드바이스 TransactionInterceptor

스프링은 트랜잭션 경계설정 어드바이스로 사용하기위해 `TransactionInterceptor` 클래스를 제공한다.  

TransactionInterceptor은 unchecked Exception(Runtime Exception을 상속받는 예외 클래스들)에 대해서만 rollback을 처리한다. checked Exception의 경우 비지니스 로직상에서 정상적인 처리 흐름이라고 본다.

## 프록시 방식 AOP적용에서의 주의사항

- 프록시 방식의 AOP에선 타깃 클래스에서 자기 자신의 내부 메서드를 호출 할 경우 AOP가 적용되지 않는다. 프록시 방식은 클라이언트에서 타깃을 사용할 때 타깃 대신 프록시 객체를 이용하도록 만들고 어드바이스를 적용시킨 후 타깃을 호출하는 방식을 이용하기 때문에 타깃 객체에서 자기 자신의 메서드를 호출하게되면 프록시가 적용되지 않기 때문이다.

### [References]

1. [https://ssmlim.tistory.com/45](https://ssmlim.tistory.com/45)