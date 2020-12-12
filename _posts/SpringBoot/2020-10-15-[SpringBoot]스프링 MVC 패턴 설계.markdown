---
layout: post
title: "[SpringBoot]스프링 MVC 패턴"
subtitle: "스프링 MVC 패턴"
categories: study
tags: springboot
---

# 스프링 MVC 패턴

![SpringMvcFlow.png](/assets/img/SpringMvcFlow.png)

## 흐름 설명

1. Spring은 `DispatcherServlet` 에서 클라이언트의 요청을 받습니다.
2. DispatcherServlet은 `HandlerMapping`을 통해 클라이언트의 요청에 일치하는 Handler(Controller)를 찾습니다.
3. DispatcherServlet은 `HandlerAdapter` 객체에 요청 처리를 위임합니다. HandlerAdapter는 Controller의 메소드를 호출해 요청을 처리하도록 합니다.
4. `Controller`는 비즈니스 로직을 담당하는 Service를 호출합니다.
   1. `Service`는 Repository 객체를 통해 DB 데이터에 접근합니다.
   2. `Repository`는 Service와 DB사이에서 데이터를 주고 받을 수 있도록 합니다.
5. 비즈니스 로직을 다 처리하고 나서 Controller는 처리 결과와 view name을 설정한 ModelAndView 객체를 HandlerAdapter에 리턴합니다.
6. DispatcherServlet은 `ViewResolver` 객체를 이용해 결과를 보여줄 뷰를 검색합니다. ViewResolver는 ModelAndView 객체의 view name을 활용해 View 객체를 찾거나 생성해서 DispatcherServlet에 리턴합니다.
7. DispatcherServlet은 ViewResolver가 리턴한 View 객체에게 응답 결과 생성을 요청합니다.
8. JSP 또는 HTML은 ModelAndView 객체의 Model 객체에 담겨있는 응답 결과 데이터를 사용해 클라이언트에게 화면을 보여줄 수 있습니다.

> JSP와 HTML의 차이?
>
> > **JSP** : 서버에서 compile을 한 후 HTML형식으로 결과을 클라이언트로 보내면 브라우저에서 interpret합니다.
> > **HTML** : 브라우저에서 HTML을 interpret합니다.
