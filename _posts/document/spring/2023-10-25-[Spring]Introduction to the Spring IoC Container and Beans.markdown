---
layout: post
title: "[Spring] Spring IoC Container, Bean 개괄"
subtitle: "IoC 컨테이나와 Bean"
categories: document
tags: spring-doc
---

> Spring 공식 문서 정리  
> 
> - Introduction to the Spring IoC Container & Beans
> - Container Overview
> - Bean Overview

## [Introduction to the Spring IoC Container and Beans](https://docs.spring.io/spring-framework/reference/core/beans/introduction.html)

### IoC (Inversion Of Control)

- 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 **외부**에서 관리

### DI

- 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것
- 장점
  - 관심사의 분리를 통한 **높은 응집도**
  - 클라이언트 코드 변경 없이, 클라이언트가 호출하는 대상의 타입 인스턴스 변경 가능
  - 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계 쉽게 변경

### 스프링에서의 IoC & DI

- IoC Container가 Bean을 생성하고 의존성을 주입
- 스프링 IoC Container 패키지 : `org.springframework.beans`, `org.springframework.context`
- **`BeanFactory`** : 기본 기능 제공
- **`ApplicationContext`** : BeanFactory 기능과 더해 엔터프라이즈 특화 기능 추가 제공
- **`Bean`** : 스프링 IoC Conatiner가 관리하는 오브젝트



## [Container Overview](https://docs.spring.io/spring-framework/reference/core/beans/basics.html)

![The Spring IoC conatiner](https://docs.spring.io/spring-framework/reference/_images/container-magic.png)

- `org.springframework.context.ApplicationContext` 인터페이스
  - 스프링 IoC Container의 대표
  - 인스턴스화, 구성, 빈들의 조합을 책임짐
  - 메타 데이터 기반 어떤 오브젝트를 빈으로 생성할지 판단.
  - 메타 데이터는 XML, 자바 어노테이션, 자바 코드 포맷 사용 가능
  - Springboot는 어떤 클래스를 사용할까?
    - `AnnotationConfigServletWebServerApplicationContext` : 서블릿 기반 웹 애플리케이션, Tomcat
    - `AnnotationConfigReactiveWebServerApplicationContext` : 리액티브 웹 애플리케이션, Reactor/Netty

### Configuration Metadata

- 스프링 컨테이너는 설정 메타데이터 사용.
- 메타데이터 : XML 기반, 최근에는 Java 기반 설정이 많이 사용됨
- Bean 정의는 서비스 계층, 영속성 계층(repository, DAO와 같은), 프레젠테이션 계층, 인프라 객체 등 정의. 도메인은 리포지토리 및 비즈니스 로직의 책임으로 Bean 설정 X

### Instantiating a Container

- XML 기반 컨테이너 인스턴스화
  
  ```java
  ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
  ```
- Spring Boot는?
  - 스프링 부트는 스프링 컨테이너 직접 인스턴스화 불필요 : `spring-boot-starter`, `spring-boot-starter-web` 과 같은 starter가 config 자동 제공
  - **`@SpringBootApplication`** 어노테이션 내 `@EnableAutoConfiguration`, `@ComponentScan`이 자동 config 설정을 위한 어노테이션 포함
  - `spring-boot-autoconfigure` 모듈 사용

### Using the container

**ApplicationContext**에서 제공하는 `T getBean(String name, Clas<T> requiredType)` 통해 bean을 가져올 수 있음

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

`getBean()`과 같은 Spring API를 직접적으로 사용하는 것은 권장하지 않음. 



## [Bean Overview](https://docs.spring.io/spring-framework/reference/core/beans/definition.html)

Beam은 클래스 이름, 행동 구성, 의존성 및 기타 설정과 같은 정보를 포함하는 [**`BeanDefinition`**](https://github.com/spring-projects/spring-framework/blob/b3a6dbaab38b7fe26ed8ef771fd282cd961f0433/spring-beans/src/main/java/org/springframework/beans/factory/config/BeanDefinition.java) 객체로 표현됨



### Naming Beans

bean 이름은 unique 해야 함. 일반적으로 식별자는 한 개이지만, alias를 사용하여 다수를 적용할 수도 있음.



Bean 명명 규칙

- Java 컨벤션을 따름

- 소문자로 시작, camel-case



Alias를 활용한 하나의 Bean에 대한 이름 설정

```java
@Configuration
public class AppConfig {

	@Bean({"dataSource", "subsystemA-dataSource", "subsystemB-dataSource"})
	public DataSource dataSource() {
		// instantiate, configure and return DataSource bean...
	}
}
```



### Instantiating Beans

생성자를 통한 인스턴스화 & 정적 팩토리 메소드를 통한 인스턴스화



### Instantiation with a Constructor

생성자를 통한 인스턴스의 경우 일반적인 JavaBean 활용 가능. IoC 사용 유형에 따라 기본 생성자가 필요한 경우도 있을 수 있으나, 대체로 간단하게 bean 생성을 적용할 수 있음.



### Instantiation with a Static Factory Method

`factory-method` 속성에 인스턴스에 필요한 팩토리 메소드를 명시하는 방식.

```xml
<bean id="clientService"
	class="examples.ClientService"
	factory-method="createInstance"/>
```

```java
public class ClientService {
	private static ClientService clientService = new ClientService();
	private ClientService() {}

	public static ClientService createInstance() {
		return clientService;
	}
}
```



### Instantiation by Using an Instance Factory Method

- xml 기반 인스턴스 factory method 사용 : not static

```xml
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
	<!-- inject any dependencies required by this locator bean -->
</bean>

<bean id="clientService"
	factory-bean="serviceLocator"
	factory-method="createClientServiceInstance"/>

<bean id="accountService"
	factory-bean="serviceLocator"
	factory-method="createAccountServiceInstance"/>
```

```java
public class DefaultServiceLocator {

	private static ClientService clientService = new ClientServiceImpl();

	private static AccountService accountService = new AccountServiceImpl();

	public ClientService createClientServiceInstance() {
		return clientService;
	}

	public AccountService createAccountServiceInstance() {
		return accountService;
	}
}
```



- Java 기반 instance facotry method

```java
@Configuration
public class DefaultServiceLocator {

	private static ClientService clientService = new ClientServiceImpl();

	private static AccountService accountService = new AccountServiceImpl();

    @Bean("clientService")
	public ClientService createClientServiceInstance() {
		return clientService;
	}

    @Bean("accountService")
	public AccountService createAccountServiceInstance() {
		return accountService;
	}
}
```



특정  Bean의 런타임 유형 확인 하는 방법 : [**`BeanFactory.getType`**](https://github.com/spring-projects/spring-framework/blob/b3a6dbaab38b7fe26ed8ef771fd282cd961f0433/spring-beans/src/main/java/org/springframework/beans/factory/BeanFactory.java#L357)



## [Reference]

- Spring doc.
  - [Introduction to the Spring IoC Container and Beans](https://docs.spring.io/spring-framework/reference/core/beans/introduction.html)
  - [Container Overview](https://docs.spring.io/spring-framework/reference/core/beans/basics.html)
  - [Bean Overview](https://docs.spring.io/spring-framework/reference/core/beans/definition.html)