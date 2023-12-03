---
layout: post
title: "[Spring] Spring docs 3주차"
subtitle: Bean 상속, 컨테이너 확장, 어노테이션 기반 설정
categories: document
tags:
  - spring-doc
---
> Spring 공식 문서 정리

>

> - Bean Definition Inheritance

> - Container Extension Points

> - Annotation-based Container Configuration


# 1.  Bean Definition Inheritance
자식 Bean은 부모 Bean의 속성 및 설정을 물려받음. 자식은 부모 속성에 대해 override하거나 추가를 할 수 있음. 템플릿의 한 종류로 볼 수 있음.


`parent` 속성을 통해 상속을 받을 수 있음.
하단 `parent="inheritedTestBean"` 참조.

```xml
<bean id="inheritedTestBean" abstract="true"
		class="org.springframework.beans.TestBean">
	<property name="name" value="parent"/>
	<property name="age" value="1"/>
</bean>

<bean id="inheritsWithDifferentClass"
		class="org.springframework.beans.DerivedTestBean"
		parent="inheritedTestBean" init-method="initialize">
	<property name="name" value="override"/>
	<!-- the age property value of 1 will be inherited from parent -->
</bean>
```


부모 bean 정의에 `class` 속성을 생략하고 `abstract="true"` 를 준 경우
- 부모 Bean 자체 인스턴스화 X
- 컨네이너 내부적으로 해당 Bean을 무시하고 진행됨
- abstract를 생략하는 경우 → ApplicationContext는 부모 Bean을 인스턴스화 시도하여 에러 발생

XML 기반의 class 속성 생략 & abstract=true 속성을 활용한 Bean Definition template은 어노테이션 기반으로는 구현 불가한 것으로 보임

# 2. Container Extension Points
ApplicationContext 구현 대신 특정 인터페이스를 통해 Spring IoC container의 확장을 할 수 있음.

## 1) BeanPostProcessor
> Bean 커스터마이징 하기

- Bean 초기화 로직, 의존 해결 등에 대한 콜백 메소드를 제공.
- 컨테이너가 Bean에 대해 인스턴스화, 설정, 초기화를 완료한 후 일부 커스텀 로직을 추가하고자 할 때 **BeanPostProcessor** 인터페이스를 활용.
- 여러 개의 BeanPostProcessor 정의 가능 : Ordered 인터페이스 구현 통해 순서 적용
- BeanPostProcessor의 적용 범위는 컨테이너
- 두 개의 메소드
	1. postProcessBeforeInitialization(Object bean, String beanName)
		- 빈이 초기화되기 전에 호출되는 메서드
		- 빈의 초기화 전에 원하는 작업 추가 가능
	2. postProcessAfterInitialization(Object bean, String beanName)
		- 빈이 초기화된 후 호출되는 메서드
		- 빈의 초기화 후 작업 추가 가능

## 2) BeanFactoryPostProcessor
- BeanPostProcessor와 전반적으로 비슷.
	- 다른 점 : BeanFactoryPostProcessor는 Bean 설정 metadata에 적용
- 주로 Bean의 프로퍼티나 설정을 동적으로 변경하거나, 외부 설정 파일을 읽어와서 Bean의 구성을 변경하는 용도로 사용
- `postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory)`

## 3) FactoryBean
- 빈을 생성, 제공하는 방법 중 하나
- `T getObject()` : 실제로 생성된 Bean 리턴.
- `Class<?> getObjectType()` : getObject() 메서드가 반환하는 빈의 타입 지정. Bean 타입 명시적 지정 가능.
- FactoryBean을 통해 생성한 bean의 id는 FactoryBean을 구현한 클래스명
	- beanFactory.getBean("&factoryBeanTest") : &를 붙이면 FactoryBean 자체를 가져올 수 있음

# 3. Annotation-based Container Configuration
XML vs 어노테이션 기반
- 어노테이션 기반
	- 짧고 간결하게 구성 가능
- XML
	- 소스 코드 건들지 않음. 다시 컴파일할 필요 없음
- Spring에서 둘 다 동시에 사용 가능


@PostContruct, @PreDestroy, @Inject, @Named 등의 어노테이션 제공됨

어노테이션 주입이 XML 주입 이전에 적용됨. XML 설정이 어노테이션 설정을 overriding 함.

## 1) @Autowired
- @Autowired 대신 @Inject와 @Named (JSR 330 Standard Annotations)도 동일하게 사용 가능
- Spring 4.3 부터 Bean의 생성자 하나만 있는 경우 @Autowired 주석 불필요
- 필드, setter 메서드(임의 메소드명도 가능), 생성자에 적용 : 각 방식 혼용도 가능
- Array, Collection 통해 해당 타입의 Bean들을 전부 주입받을 수도 있음
	- @Order, @Priority 통해 배열 및 collection element의 순서를 정할 수도 있음
- `Map<String/*bean name*/, Object/*bean*/>` 형식으로도 주입 받을 수 있음
- `Optional<? extends Class>` 로 받아 Bean 존재 여부도 체크할 수 있음
- Spring 5.x 부터 `@Nullable`  사용 가능
```java
public class SimpleMovieLister {

	@Autowired
	public void setMovieFinder(@Nullable MovieFinder movieFinder) {
		...
	}
}
```

## 2) @Primary
여러 빈 후보군들이 있을 때 단일 Bean을 주입받는 경우 어떻게 처리할 것인가?
Spring에서는 `@Primary` 를 제공하고 있음.
Bean 정의하는 부분에 @Primary를 붙이면 됨

## 3) @Qualifier
@Primary 대신 @Qualifier("beanName")통해 주입 받을 수도 있음
@Qualifier에 없는 Bean id를 기입하면? 에러 발생

## 4) CustomAutoConfigurer
Autowiring 관련 설정을 사용자 정의하기 위해 확장할 수 있는 클래스.
특정 타입의 빈을 자동으로 와이어링하는 방식을 사용자가 제어 가능.

## 5) @Resource
@Resource는 JSR-250 어노테이션으로, @Autowired와 동일

## 6) @Value
- 외부 property 주입할 때 사용
- @Value("${test.name:default}") : default 값 설정 가능
- [SpEL](https://docs.spring.io/spring-framework/reference/core/expressions.html) 사용 가능
## 7)  @PostConstruct, @PreDestroy
JSR-250 의 @PostConstruct, @PreDestroy
- @PostConstruct
	- Spring에서 빈이 초기화된 후 수행할 메서드
	- void 타입, 매개변수 없어야 함
	- 빈이 생성되고 의존성이 주입된 후 호출
	- 빈의 초기화 로직 담는데 유용
- @PreDestroy
	- Spring에서 빈이 소멸되기 전에 실행될 메서드
	- 주로 자원 해제, 연결 종료 등 정리 작업에 활용