---
layout: post
title: "[Spring] Spring AOP"
subtitle: "spring aop"
categories: study
tags: springboot
---
> Spring AOP

# AOP (Aspect Oriented Programming)
관점지향 프로그래밍. 객체지향 프로그래밍은 하나의 관심사로 응집되어 행위를 제공하고 다른 객체와 협력하도록 설계하고 개발하는 사상이다. 그러나 애플리케이션의 핵심 비즈니스 로직은 아니지만 애플리케이션을 구성하는 중요한 요소가 되는 부가기능을 응집시켜야 할 필요가 생겼다.  

<br/>

스프링의 트랜잭션 경계설정과 같은 작업은 비즈니스 로직과는 상관이 없으나, 애플리케이션 레이어와 DB 연결을 수행하는 인프라 레이어가 연동이 되고, 연결이 종료되는 과정에서 꼭 필요한 중요한 요소 중 하나이다.

<br/>

결국 비즈니스 로직을 감싸서 부가기능을 추가할 수 있는 프록시가 필요하게 되었고, AOP는 이러한 개념을 `어드바이스`와 `포인트컷`을 통해 실현했다. 

<br/>

AOP 또한 한 가지 관심사로 응집하여 객체가 활동할 수 있어야 하는 OOP를 제대로 구현하도록 만들어주기 위한 보조적인 역할을 수행한다고 볼 수 있다.

## Spring AOP
스프링은 다이내믹 프록시, 데코레이터 패턴, 프록시 패턴, 자동 프로시 생성 기법, 빈 오브젝트 후처리 조작 기법등을 통해 AOP를 지원한다.

<br/>

그러나 AspectJ의 경우 프록시 방법이 아닌 .class(자바 바이트코드)가 JVM에 로딩되는 시점을 가로채서 바이트코드 자체를 변경시켜 어드바이스를 포인트컷에 적용시키는 방법을 이용한다.

<br/>

스프링의 AOP는 다이내믹 프록시 방법을 이용하기 때문에 메소드의 실행 지점만 조인 포인트로 쓸 수 있다.

<br/>

다이내믹 프록시는 클라이언트에게 타깃 객체가 아닌 타깃 객체와 동일 타입인 프록시를 반환해준다. 따라서 클라이언트는 타깃 객체인줄알고 해당 메서드를 실행하지만 사실은 프록시 객체가 해당 메서드 호출을 받게되고 그 과정에서 부가 기능을 수행한 뒤 원래 타깃 객체에게 흐름을 위임하는 방식을 사용하는것이다. 

<br/>

따라서 스프링 AOP에서는 타깃 메서드를 실행(사실은 프록시 객체의 메서드를 실행)하는 순간에만 조인포인트를 걸 수 있다.

<br/>

또한 AOP를 걸어놓은 타깃 객체에서 자기 자신의 메서드를 호출하게되면 프록시를 타지않기 때문에 AOP가 수행되지않는다.

## AOP 용어
- 타깃
    - 부가기능을 부여할 대상
- 어드바이스
    - 타깃에 제공할 부가기능을 담은 모듈
- 조인 포인트
    - 어드바이스가 적용될 수 있는 위치. 스프링의 프록시 AOP에서 조인 포인트는 메소드의 실행 단계 뿐이다.
- 포인트컷
    - 어드바이스를 적용할 조인포인트를 선별하는 작업.
- 프록시
    - 클라이언트와 타깃 사이에서 투명하게 존재하면서 부가기능을 제공하는 오브젝트. DI를 통해 클라이언트에 주입되며, 클라이언트가 타깃의 메소드를 호출하고자 할때 해당되는 호출을 대신 받아 부가기능을 앞뒤로 수행하고 타깃에 위임한다.
- 어드바이저
    - 포인트컷과 어드바이스를 하나씩 갖고있는 오브젝트.  스프링 AOP에서만 사용하는 용어다
- 애스팩트
    - 한 개 이상의 포인트컷과 어드바이스의 조합으로 만들어지며, 보통 싱글톤 형태의 오브젝트로 존재한다.

## Spring Dynamic Proxy
```java
@SuppressWarnings("serial")
public class ProxyFactory extends ProxyCreatorSupport {

	public ProxyFactory() {
	}
	public ProxyFactory(Object target) {
		setTarget(target);
		setInterfaces(ClassUtils.getAllInterfaces(target));
	}
	public ProxyFactory(Class<?>... proxyInterfaces) {
		setInterfaces(proxyInterfaces);
	}
	public ProxyFactory(Class<?> proxyInterface, Interceptor interceptor) {
		addInterface(proxyInterface);
		addAdvice(interceptor);
	}
	public ProxyFactory(Class<?> proxyInterface, TargetSource targetSource) {
		addInterface(proxyInterface);
		setTargetSource(targetSource);
	}

	public Object getProxy() {
		return createAopProxy().getProxy();
	}
	public Object getProxy(@Nullable ClassLoader classLoader) {
		return createAopProxy().getProxy(classLoader);
	}

	@SuppressWarnings("unchecked")
	public static <T> T getProxy(Class<T> proxyInterface, Interceptor interceptor) {
		return (T) new ProxyFactory(proxyInterface, interceptor).getProxy();
	}
	@SuppressWarnings("unchecked")
	public static <T> T getProxy(Class<T> proxyInterface, TargetSource targetSource) {
		return (T) new ProxyFactory(proxyInterface, targetSource).getProxy();
	}
	public static Object getProxy(TargetSource targetSource) {
		if (targetSource.getTargetClass() == null) {
			throw new IllegalArgumentException("Cannot create class proxy for TargetSource with null target class");
		}
		ProxyFactory proxyFactory = new ProxyFactory();
		proxyFactory.setTargetSource(targetSource);
		proxyFactory.setProxyTargetClass(true);
		return proxyFactory.getProxy();
	}

}
```

## Dynamic Proxy 방법

1. JDK Dynamic Proxy

    DI받은 객체가 인터페이스 일 경우 JDK 다이나믹 프록시를 이용하여 프록시 객첼르 만들어 AOP를 구현한다.

    자바의 reflection을 이용한 다이나믹 프록시는 interface에만 국한되어 있다. 구체 구현클래스는 이 방법을 쓰지 못한다.

2. CGLIB 라이브러리를 이용한 Dynimic proxy

    DI받은 객체가 구체 구현 클래스 일 경우 CGLIB라이브러리를 이용하여 다이나믹 프록시를 만들도록 한다. 이 경우 final로 생성한 타입이거나 메서드 일 경우 advice 적용이 불가능한 제약사항이 있다.

스프링의 DI는 인터페이스를 통해 객체간의 의존성을 낮춰주어야 한다. 구체 구현클래스를 넣어도 DI가 되며 정상 작동하지만 class에서 public field를 선언하는 것 처럼 좋은 방법이라 볼 수 없다.