---
layout: post
title: "[Spring]Servelet & Spring-webmvc"
subtitle: "servlet & spring"
categories: study
tags: springboot
---

> Servlet 과 Spring

# Servelet & Spring
## 1. Servlet
- HTML 등의 웹 컨텐츠를 생성하고 전달하기 위해 Servlet 클래스의 구현 규칙을 지켜 자바로 만들어진 `프로그램`

### 1-1. 작동방식
- 각각의 Servlet이 Servlet Interface를 구현
- Servlet Container가 Servlet을 관리하며 서버와 통신을 함

### 1-2. 나오게 된 배경
- 과거에는 static 파일만 제공
- 동적인 동작을 하는 페이지 필요해짐
- 서버와 통신하기 위한 규약이 필요해짐 -> `CGI`가 나오게 됨

### 1-3. CGI?
> http 통신 규약을 사용하는 웹서버가 프로그램(웹 애플리케이션)과 데이터를 주고 받는 처리 규약
- 인터페이스로 여러 언어로 구현 가능
- Servlet 이전에는 직접 구현
- CGI 단점
  + 1 Client - 1 Process
  + 동일 CGI 구현체이더라도 매번 새로 생성

### 1-4. Servlet
- Servlet도 CGI 규칙에 따라 데이터를 주고 받지만 이를 서블릿을 가지고 있는 `컨테이너에게 위임`하고 대신 서블릿 컨테이너와 서블릿 사이의 규칙이 존재
- 하나의 요청에서 하나의 프로세스가 껐다 켜졌다 하는 기존 CGI의 구조가 X
- 서블릿이 컨테이너 내부에서 쓰레드 단위로 요청을 처리하고 생명 주기를 가짐
- 흐름은 개발자가 아니라 컨테이너가 제어함(IoC)
- 서블릿은 웹서버 내부에서 동작하는 작은 자바 프로그램
- 일반적으로 http를 통해 웹 클라이언트들로부터 요청을 수신하고 응답
- Servlet 인터페이스를 구현하기 위해 GenericServlet을 상속 받거나 HTTP Servlet을 상속받아 구현

#### Servlet의 Life Cycle
1. init() 
  - Servlet 인스턴스 생성
2. Client로부터 발생한 모든 요청은 service() 를 통해 다룸
  - 실제 기능이 수행되는 곳
  - HTTP method (GET, POST, PUT, DELETE) 에 따라 doGet(), doPost(), doPut(), doDelete() 메서드를 호출
3. Servlet이 서비스를 마치게 되면, destroy()를 통해 종료되며 GC에 의해 메모리 정리됨
  - 서블릿 인스턴스 사라질 때 호출됨
  - 보통 컨테이너가 종료되는 시점에 destroy() 호출
  - 특정 서블릿 로드/언로드 시에도 사용
- 이미 생성된 Servlet 객체는 메모리에 남겨두어 동일한 Servlet에 대해 다시 init()을 실행하지 않고 재사용
- 

```java
public interface Servlet {
    void init(ServletConfig var1) throws ServletException;

    ServletConfig getServletConfig();

    void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;

    String getServletInfo();

    void destroy();
}
```

## 2. Spring
- 자바 엔터프라이즈 개발을 편리하게 해주는 오픈소스 경량급 애플리케이션 `프레임워크`

#### Spring-webmvc의 DispatcherServlet 상속구조
- Servlet 
- GenericeServlet
- HttpServlet : 여기까지가 톰캣
- HttpServletBean : 스프링
- FrameworkServlet : 스프링
- DispatcherServlet : 스프링



### 공부하면서..
해당 자료를 공부하면서 우연히 Spring-webmvc를 직접 열어보았더니, DispatcherServlet의 doDispatch 메서드에 하드코딩 되어 있는 부분을 발견해서 enum을 적용한 수정본을 Pull Request를 Spring에 날리기도 했다.  
 Spring-webmvc는 내부적으로 spring-web을 의존하고 있기 때문에 HttpMethod enum을 사용할 수 있다고 생각했기 때문에 기존 "GET", "HEAD"로 되어 있던 하드 코딩을 spring-mvc의 HttpMethod enum 타입으로 적용했다.  
 PR을 받아줄지는 모르겠지만 그래도 뭔가 신기하고 재밌는 경험이었다.  
 [https://github.com/spring-projects/spring-framework/pull/26855](https://github.com/spring-projects/spring-framework/pull/26855)

