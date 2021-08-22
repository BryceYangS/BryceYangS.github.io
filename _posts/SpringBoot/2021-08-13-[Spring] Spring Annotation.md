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
- **@Bean** : 스프링 컨텍스트에서 관리되며, **메서드에서 반환된 결과**를 빈으로 지정하는 애너테이션. 반환된 빈은 팩토리 메서드와 동일한 이름을 가진다.
- **@Lookup** : tells Spring to return an instance of the method's return type when we invoke it.
- **@Primary** : 동일 타입의 빈이 다수 존재할 때, 특정 빈에 **우선순위**를 높게 준다.
- **@Required** : shows that the setter method must be configured to be dependency-injected with a value at configuration time. Use @Required on setter methods to mark dependencies populated through XML. Otherwise, a BeanInitializationException is thrown.
- **@Value** : used to assign values into fields in Spring-managed beans. It's compatible with the constructor, setter, and field injection.
- **@DependsOn** : makes Spring initialize other beans before the annotated one. Usually, this behavior is automatic, based on the explicit dependencies between beans. The @DependsOn annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with @Bean.
- **@Lazy** : makes beans to initialize lazily. @Lazy annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with @Bean.
- **@Scope** : @Component 클래스 또는 @Bean의 범위(Scope)를 정의하기 위해 사용된다. 싱글턴(singleton), 프로토타입(prototype), 리퀘스트(request), 세션(session), 글로벌 세션(globalSession), 커스텀 스코프.
    - default 는 싱글턴 : 객체 한 개만 생성
    - 프로토타입 : 매 번 객체 생성
- **@Profile** : 특정 프로파일에만 해당 빈이 추가된다.

## 🌗중요한 스프링 부트 애너테이션
### @SpringBootApplication
One of the most basic and helpful annotations, is @SpringBootApplication. It's syntactic sugar for combining other annotations that we'll look at in just a moment. @SpringBootApplication is **@Configuration**, **@EnableAutoConfiguration** and **@ComponentScan** annotations combined, configured with their default attributes.


### @Configuration & @ComponentScan
The @Configuration and @ComponentScan annotations that we described above make Spring create and configure the beans and components of your application. It's a great way to decouple the actual business logic code from wiring the app together.


### @EnableAutoConfiguration
Now the @EnableAutoConfiguration annotation is even better. It makes Spring guess the configuration based on the JAR files available on the classpath. It can figure out what libraries you use and preconfigure their components without you lifting a finger. It is how all the spring-boot-starter libraries work. Meaning it's a major lifesaver both when you're just starting to work with a library as well as when you know and trust the default config to be reasonable.


## 🌕중요한 스프링 MVC Web 애너테이션
- **@Controller** : marks the class as a web controller, capable of handling the HTTP requests. Spring will look at the methods of the class marked with the @Controller annotation and establish the routing table to know which methods serve which endpoints.
- **@ResponseBody** : The @ResponseBody is a utility annotation that makes Spring bind a method's return value to the HTTP response body. When building a JSON endpoint, this is an amazing way to magically convert your objects into JSON for easier consumption.
- **@RestController** : Then there's the @RestController annotation, a convenience syntax for @Controller and @ResponseBody together. This means that all the action methods in the marked class will return the JSON response.
- **@RequestMapping(method = RequestMethod.GET, value = "/path")** : The @RequestMapping(method = RequestMethod.GET, value = "/path") annotation specifies a method in the controller that should be responsible for serving the HTTP request to the given path. Spring will work the implementation details of how it's done. You simply specify the path value on the annotation and Spring will route the requests into the correct action methods.
- **@RequestParam(value="name", defaultValue="World"** : Naturally, the methods handling the requests might take parameters. To help you with binding the HTTP parameters into the action method arguments, you can use the @RequestParam(value="name", defaultValue="World") annotation. Spring will parse the request parameters and put the appropriate ones into your method arguments.
- **@PathVariable("placeholderName")** : Another common way to provide information to the backend is to encode it in the URL. Then you can use the @PathVariable("placeholderName") annotation to bring the values from the URL to the method arguments.


## 🌓마무리

## 🌒References
- [https://www.jrebel.com/blog/spring-annotations-cheat-sheet](https://www.jrebel.com/blog/spring-annotations-cheat-sheet)