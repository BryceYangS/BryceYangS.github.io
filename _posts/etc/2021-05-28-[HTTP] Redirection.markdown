---
layout: post
title: "[HTTP] Redirection"
subtitle: "Redirection"
categories: study
tags: etc
---
> HTTP Redirection

### 🚀3xx (Redirection)
> 요청을 완료하기 위해 유저 에이전트(웹 브라우저)의 추가조치 필요  

웹 브라우저는 3xx 응답의 결과에 `Location 헤더`가 있으면, Location 위치로 자동 이동  

### 🚀Redirection 종류 3가지
#### 💻1. 영구 리다이렉션
- 특정 리소스의 URI가 영구적으로 이동
- 원래의 URL을 사용❌, 검색 엔진 등에서도 변경 인지
- HTTP 상태 코드
    1. `301` Moved Permanently
        - 리다이렉트시 요청 메서드가 **GET**으로 변하고, 본문이 제거될 수 있음
    2. `308` Permanent Redirect
        - 301과 기능 동일
        - 리다이렉트시 요청 메서드와 본문 유지
- ie) /members → /users
- ie) /event → /new-event

#### 💻2. 일시 리다이렉션
일시적인 변경  
- 리소스의 URI가 일시적으로 변경
- 검색 엔진 등에서 URL을 변경하면 안됨
- HTTP 상태 코드
    1. `302` Found
        - 리다이렉트시 요청 메서드가 **GET**으로 변하고, 본문이 제거될 수 있음
    2. `307` Temporary Redirect
        - 302와 기능 동일
        - 리다이렉트시 요청 메서드와 본문 유지(요청 메서드를 변경하면 안된다)
    3. `303` See Other
        - 302와 기능 동일
        - 리다이렉트시 요청 메서드가 GET으로 변경
- PRG : Post/Redirect/Get
    - POST로 주문 후에 새로 고침으로 인한 중복 주문 방지
    - POST로 주문 후에 주문 결과 화면을 GET 메서드로 리다이렉트
    - 새로고침해도 결과 화면을 GET으로 조회
    - 중복 주문 대신 결과 화면만 GET으로 다시 요청
- 302 ? 307 ? 303 ? 어떤 것 사용?
    - 302 : GET으로 변경 될 수 있음
    - 307 : 메서드가 변하면 안 됨
    - 303 : 메서드가 GET으로 변경
    - 원칙
        - 302 스펙의 의도는 HTTP 메서드 유지, 허나 웹 브라우저 대부분 302 코드에 대해 GET으로 변경
        - 모호한 302 대신 307, 303 등장
    - 현실
        - 구체적인 307, 303 사용을 권장하나 많은 애플리케이션 라이브러리들이 302 기본 사용
        - 자동 리다이렉션시 GET으로 변해도 되면 302 사용해도 큰 문제 ❌

#### 💻3. 특수 리다이렉션
결과 대신 `캐시`를 사용  
- HTTP 상태 코드
    1. `300` Multiple Choices
        - 사용 ❌
    2. `304` Not Modified
        - 캐시를 목적으로 사용
        - 클라이언트에게 리소스가 수정되지 않았음을 알려줌. 따라서 클라이언트는 로컬 PC에 저장된 캐시를 재사용(캐시로 리다이렉트)
        - 304 응답은 응답에 메시지 바디를 포함하면 ❌ (로컬 캐시 사용해야 하기 때문)
        - 조건부 GET, HEAD 요청시 사용

### 🚀2.2.참조
- [https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections](https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections) 
- 인프런, 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC#)