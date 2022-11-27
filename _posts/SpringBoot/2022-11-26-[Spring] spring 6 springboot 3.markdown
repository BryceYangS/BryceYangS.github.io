---
layout: post
title: "[Spring] Spring 6 & SpringBoot 3"
subtitle: "spring 6, springboot 3"
categories: study
tags: springboot
---

> Spring 6.x & SpringBoot 3.x Release

# 1. Spring 6.x
## Java 17 기반
- Java 17 : records, instanceof, multiline string 등
- JDK 19 Virtual Thread 지원 (Project Loom)
    - https://howtodoinjava.com/java/multi-threading/virtual-threads/
    - https://spring.io/blog/2022/10/11/embracing-virtual-threads

## Java EE -> Jakarta EE 로 대체
- Jakarta EE9 ~ 지원
- javax 패키지 -> jakarta 패키지로 변경

## HttpClient
- [spring-docs](https://docs.spring.io/spring-framework/docs/6.0.0-RC1/reference/html/integration.html#rest-http-interface)
- OpenFeign과 같이 어노테이션 기반 클라이언트 라이브러리
- `@HttpExchange`

## ProblemDetail API
- HTTP API 응답에 대한 라이브러리 적용
- [spring-docs](https://docs.spring.io/spring-framework/docs/6.0.0-RC1/reference/html/web.html#mvc-ann-rest-exceptions)
- [RFC-7807](https://www.rfc-editor.org/rfc/rfc7807.html)
- example : https://howtodoinjava.com/spring-mvc/spring-problemdetail-errorresponse/

## AOT (Ahead-Of-Time) transformation
- [youtube Spring Tips: the road to Spring Boot 3: ahead-of-time compilation and GraalVM](https://www.youtube.com/watch?v=TOfYlLjXufw)
- Spring AOT?
    - 빌드 시 스프링 애플리케이션을 분석하고 최적화하는 도구
    - AOT 엔진은 GraalVM Native Configuration이 필요로 하는 reflection configuration을 생성해줌
    - spring native 파일로 컴파일 하는 데 사용되고 이 후 애플리케이션의 시작 시간과 메모리 사용량을 줄여줌
![](/assets/img/springboot/springboot_aot.png)
![](/assets/img/springboot/springboot_aot_detail.png)

# 2. SpringBoot 3.x
- spring 6.x 기반

## Deprecated Feature 제거
- `spring.config.use-legacy-processing=true` 제거

## Springboot 버전 업그레이드 하기
- Spring-boot-migrator 사용 가능 : https://github.com/spring-projects-experimental/spring-boot-migrator
    - spring boot 2.7 -> 3.0 업그레이드 지원
    - 2.6 -> 2.7 도 지원

## Springboot 수동 업그레이드 하기
1. JDK 17 업그레이드
2. spring 5 / spring boot 2 최신 버전으로 업그레이드
    - deprecated 제거 필요 : sprinb boot 3에서 제거되기 때문
3. spring 6 / spring boot 3 업그레이드
4. import 문 수정 : `javax.` -> `jakarta.*`
    - `javax.persistence` -> `jakarta.persistence`
        - Hibernate ORM 5.6.x ~ : jakarta 지원
        - Spring Boot 3 ~ : Hibernate 6 / Flyway 9 사용
    - Jakarta EE9 지원하는 Tomcat, Jetty, Undertow 버전 업그레이드
    - `javax.servlet` -> `jakarta.servlet`
5. API 스펙 변경
    - Spring MVC, Webflux에서 더이상 type 레벨에서 @RequestMapping 자동 탐색 X
        - interface에서 @RequestMapping 더 이상 탐색 X
        - @Controller / @RestController 어노테이션 붙여야 사용 가능
        - spring-cloud-openfeign에서도 interface 레벨 @RequestMapping 지원 종료 : [Remove support for @RequestMapping over a FeignClient interface #547](https://github.com/spring-cloud/spring-cloud-openfeign/issues/547)
    - RetTemplate 사용하는 경우 : Apache HttpClient 5 버전 업그레이드
    - HttpMethod 변경 : `enum` -> `class`
        - [Git issue](https://github.com/spring-projects/spring-framework/issues/27697)
    - Log4j2 버전 업그레이드


# 참조
- [What’s New in Spring 6 and Spring Boot 3
](https://howtodoinjava.com/java/whats-new-spring-6-spring-boot-3/)
- [Spring 6.x wiki](https://github.com/spring-projects/spring-framework/wiki/What%27s-New-in-Spring-Framework-6.x)
- Spring 6.x 업그레이드 Wiki : https://github.com/spring-projects/spring-framework/wiki/Upgrading-to-Spring-Framework-6.x
- https://marrrang.tistory.com/101?category=925235
- https://velog.io/@jaehyunup/%EB%8B%A4%EA%B0%80%EC%98%A4%EB%8A%94-Spring-6.0-Boot-3.0-%EB%A6%B4%EB%A6%AC%EC%A6%88%EC%97%90%EC%84%9C%EC%9D%98-%EC%A3%BC%EB%AA%A9%ED%95%A0%EB%A7%8C%ED%95%9C-%EB%B3%80%EA%B2%BD