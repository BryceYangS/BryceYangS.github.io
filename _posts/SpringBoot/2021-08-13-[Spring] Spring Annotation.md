---
layout: post
title: "[Spring] 유용한 Spring Annotation"
subtitle: "스프링 애너테이션"
categories: study
tags: springboot
---
> 유용한 Spring Annotation (스프링 애너테이션)과 관련된 포스팅을 번역한 글입니다.  

# 스프링과 관련된 중요한 애너테이션
[🌘중요한 스프링 애너테이션](#중요한-스프링-애너테이션)  
[🌗중요한 스프링 부트 애너테이션](#중요한-스프링-부트-애너테이션)  
[🌕중요한 스프링 MVC Web 애너테이션](#중요한-스프링-MVC-Web-애너테이션)  
[🌓마무리](#마무리)  
[🌒References](#References)  

## 🌘중요한 스프링 애너테이션
- **@Configuration** : 빈(Bean) 정의의 소스로서 쓰이는 클래스 마크를 할 때 사용. 빈은 함께 연결되길 원하는 시스템의 컴포넌트들이다. @Bean 애너테이션이 붙은 메서드는 빈을 생성해준다. 스프링은 당신을 위해 빈의 생애 주기를 관리해주고, 빈을 생성하기 위해 앞의 메서드를 활용할 것이다.
- **@ComponentScan** : 스프링이 당신의 Configuration 클래스들을 알아내고, 빈들을 올바르게 초기화 할 수 있는지 확인하는 애너테이션. Spring이 @Configuration 클래스에 대해 구성된 패키지를 스캔하도록 한다.
- **@Import** :  만약 당신이 Configruation 클래스들에 정확한 제어가 필요하다면 추가적인 구성을 로드하기 위해 @import를 사용할 수 있다. xml 파일의 빈을 지정하는 경우에도 사용할 수 있다.
- **@Component** : 클래스에 @Component 애너테이션을 추가해 빈을 선언하는 또다른 방식. 자동 스캔 시점에 해당 클래스가 스프링 빈으로 등록된다.
- **@Service** : @Component의 특수화된 애너테이션. 일반적인 컴포넌트들보다 더 자유롭게 다루는 것이 안전하다. 서비스는 캡슐화된 상태를 가지지 않는다.
- **@Autowired** : 애플리케이션의 부품들을 연결하기 위해, 필드/생성자/메서드에 @Autowired를 사용하라. 스프링의 의존성 주입(Dependency Injection, DI)은 @Autowired가 붙어있는 클래스 멤버에 적절한 빈들을 주입하는 방식으로 이루어진다.
- **@Bean** : 스프링 컨텍스트에서 관리되며 반환된 빈을 지정하는 메서드 수준의 애너테이션. 반환된 빈은 팩토리 메서드와 동일한 이름을 가진다.
A method-level annotation to specify a returned bean to be managed by Spring context. The returned bean has the same name as the factory method.
- **@Lookup** : tells Spring to return an instance of the method's return type when we invoke it.
- **@Primary** : gives higher preference to a bean when there are multiple beans of the same type.
- **@Required** : shows that the setter method must be configured to be dependency-injected with a value at configuration time. Use @Required on setter methods to mark dependencies populated through XML. Otherwise, a BeanInitializationException is thrown.
- **@Value** : used to assign values into fields in Spring-managed beans. It's compatible with the constructor, setter, and field injection.
- **@DependsOn** : makes Spring initialize other beans before the annotated one. Usually, this behavior is automatic, based on the explicit dependencies between beans. The @DependsOn annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with @Bean.
- **@Lazy** : makes beans to initialize lazily. @Lazy annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with @Bean.
- **@Scope** : used to define the scope of a @Component class or a @Bean definition and can be either singleton, prototype, request, session, globalSession, or custom scope.
- **@Profile** : adds beans to the application only when that profile is active.

## 🌗중요한 스프링 부트 애너테이션
### @SpringBootApplication
### @Configuration & @ComponentScan
### @EnableAutoConfiguration

## 🌕중요한 스프링 MVC Web 애너테이션
- **@Controller** : 
- **@ResponseBody** : 
- **@RestController** : 
- **@RequestMapping(method = RequestMethod.GET, value = "/path")** : 
- **@RequestParam(value="name", defaultValue="World"** : 
- **@PathVariable("placeholderName")** : 


## 🌓마무리

## 🌒References
- [https://www.jrebel.com/blog/spring-annotations-cheat-sheet](https://www.jrebel.com/blog/spring-annotations-cheat-sheet)