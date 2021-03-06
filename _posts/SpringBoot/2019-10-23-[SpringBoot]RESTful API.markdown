---
layout: post
title: "[SpringBoot]RESTful API"
subtitle: "RESTful API?"
categories: study
tags: springboot
---

## RESTful API

### 1. REST란?

> _잘 표현된 HTTP URI로 리소스를 정의하고, HTTP 메서드로 리소스에 대한 행위를 정의. 리소스는 JSON, XML 등의 여러 방식으로 표현할 수 있다._

- Representational State Transfer 의 줄임말
- 웹 서비스를 만들기 위해 사용되어지는 제약조건들의 집합을 정의하는 소프트웨어 구성 방식.
- 분산 시스템 설계를 위한 아키텍쳐 스타일.
- 아키텍처 제약 조건

1. **Client-server architecture**
   -- 유저인터페이스를 데이터 스토리지와 분리
   -- 멀티 플랫폼 : 데이터 처리와 클라이언트를 분리시켜 여러 클라이언트에서 데이터에 대한 출력이 가능해짐)

2. **Stateless**: 각 요청에 클라이언트의 context가 서버에 저장되어서는 안된다.

3. **Cacheable**
   클라이언트는 응답을 캐싱할 수 있어야 한다.

4. **Layered System**
   클라이언트는 서버에 직접 연결되었는지 미들웨어에 연결되었는지 알 필요가 없어야 한다.

5. **Code on demand(option)**
   서버에서 코드를 클라이언트에게 보내서 실행하게 할 수 있어야 한다.

6. **uniform interface**
   자원은 유일하게 식별가능해야하고, HTTP Method로 표현을 담아야 하고, 메세지는 스스로를 설명(self-descriptive)해야하고, 하이퍼링크를 통해서 애플리케이션의 상태가 전이(HATEOAS)되어야 한다.

### 2. REST 구성요소

- URI : 리소스 / Method : 행위 / MIME TYPE : 표현방식  
  `1 | GET /100 HTTP/1.1 Host : https://bryceyangs.github.io`

**※참고
[HTTP 메서드]**

| Http메서드 | CRUD   | 역할                   |
| ---------- | ------ | ---------------------- |
| POST       | Create | 리소스를 생성          |
| GET        | Read   | 해당 URI의 리소스 조회 |
| PUT        | Update | 해당 URL의 리소스 수정 |
| DELETE     | Delete | 해당 URI의 리소스 삭제 |

**[URL? URI?]**

_URI_

- Uniform Resource Identifier
- 인터넷 상의 자원을 식별하기 위한 문자열

_URL_

- Uniform Resource Locator
- 인터넷 상의 자원 위치

```
URI가 가장 큰 개념, URL은 하위 개념
```

Reference:
https://jeong-pro.tistory.com/180
https://en.wikipedia.org/wiki/Representational_state_transfer
https://meetup.toast.com/posts/92
