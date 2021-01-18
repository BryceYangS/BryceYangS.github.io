---
layout: post
title: "[SpringBoot]RestTemplate"
subtitle: "RestTemplate"
categories: study
tags: springboot
---

> RestTemplate

## RestTemplate
- 스프링 프레임워크에서 제공하는 REST 서비스의 endpoint 호출하는 기능 제공. `동기`, `비동기` REST Client 제공.
- 특징
    + 기계적이고 반복적인 코드를 줄여줌
    + RESTful 형식
    + json, xml 쉽게 응답받음

### RestTemplate
- Spring 3부터 지원. REST API 호출 후 응답 때까지 대기하는 `동기` 방식

### AsycRestTemplate
- Spring 4부터 지원. `비동기` RestTemplate
- Spring 5.0부터는 `deprecated`(비추천) 됨

### WebClient
- Spring 5에 추가된 논블럭, 리액티브 웹 클라이언트로 동기, 비동기 방식 지원
<br/>
<br/>
<br/>

RestTemplate은 스프링에서 제공하는 여러 *Template 클래스들(JdbcTemplate, RedisTemplate)과 `동일한 원칙`에 따라 설계되어 단순한 방식의 호출로 복잡한 작업을 쉽게 할 수 있게 만들어줌.  
RestTmplate 클래스는 REST 서비스를 호출하도록 설계되어 `HTTP 프로토콜의 메서드`(GET, POST, PUT ,DELETE)별 여러 메서드 제공.

## RestTemplate 동작원리
org.springframework.http.client 패키지에 있다. HttpClient는 HTTP를 사용하여 통신하는 범용 라이브러리이고, `RestTemplate`은 HttpClient 를 추상화(HttpEntity의 json, xml 등)해서 제공해준다. 따라서 내부 통신(HTTP 커넥션)에 있어서는 Apache HttpComponents 를 사용한다. 만약 RestTemplate 가 없었다면, 직접 json, xml 라이브러리를 사용해서 변환해야 했을 것이다.  

![RestTemplat](/assets/img/springboot/restTemplate.png)  

1. 어플리케이션이 RestTemplate를 생성하고, URI, HTTP메소드 등의 헤더를 담아 요청한다.
2. RestTemplate 는 HttpMessageConverter 를 사용하여 requestEntity 를 요청메세지로 변환한다.
3. RestTemplate 는 ClientHttpRequestFactory 로 부터 ClientHttpRequest 를 가져와서 요청을 보낸다.
4. ClientHttpRequest 는 요청메세지를 만들어 HTTP 프로토콜을 통해 서버와 통신한다.
5. RestTemplate 는 ResponseErrorHandler 로 오류를 확인하고 있다면 처리로직을 태운다.
6. ResponseErrorHandler 는 오류가 있다면 ClientHttpResponse 에서 응답데이터를 가져와서 처리한다.
7. RestTemplate 는 HttpMessageConverter 를 이용해서 응답메세지를 java object(Class responseType) 로 변환한다.
8. 어플리케이션에 반환된다.

## RestTemplate 사용

**주요 메서드**
|메서드|HTTp|설명|
|--|--|--|
|getForObject|GET|주어진 URL 주소로 HTTP GET 메서드로 객체로 결과를 반환받는다|
|getForEntity|GET|주어진 URL 주소로 HTTP GET 메서드로 결과는 ResponseEntity로 반환받는다|
|postForLocation|POST|POST 요청을 보내고 결과로 헤더에 저장된 URI를 결과로 반환받는다|
postForObject|POST|POST 요청을 보내고 객체로 결과를 반환받는다|
|postForEntity|POST|POST 요청을 보내고 결과로 ResponseEntity로 반환받는다|
|delete|DELETE|주어진 URL 주소로 HTTP DELETE 메서드를 실행한다|
|headForHeaders|HEADER|헤더의 모든 정보를 얻을 수 있으면 HTTP HEAD 메서드를 사용한다|
|put|PUT|주어진 URL 주소로 HTTP PUT 메서드를 실행한다|
|patchForObject|PATCH|주어진 URL 주소로 HTTP PATCH 메서드를 실행한다|
|optionsForAllow|OPTIONS|주어진 URL 주소에서 지원하는 HTTP 메서드를 조회한다|
|exchange|any|HTTP 헤더를 새로 만들 수 있고 어떤 HTTP 메서드도 사용가능하다|
|execute|any|Request/Response 콜백을 수정할 수 있다|

[References]
- advenoh : [스프링 RestTemplate](https://advenoh.tistory.com/46)
- 빨간색코딩 : [RestTemplate (정의, 특징, URLConnection, HttpClient, 동작원리, 사용법, connection pool 적용)](https://sjh836.tistory.com/141)