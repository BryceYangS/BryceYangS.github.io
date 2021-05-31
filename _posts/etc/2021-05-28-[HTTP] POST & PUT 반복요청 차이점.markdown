---
layout: post
title: "[HTTP] POST & PUT 반복요청 차이점"
subtitle: "POST, PUT"
categories: study
tags: etc
---

> HTTP Method 중 POST와 PUT의 차이점

## POST & PUT 동일 요청 반복 경우 어떠한 결과가 나오는가
### 🚀idempotent?
> f(x) = f(f(x))  

우리나라 말로 멱등법칙 또는 멱등성이라 한다. 연상을 여러 번 적용하더라도 결과가 달라지지 않는 성질을 의미한다.  
POST와 PUT의 경우 멱등성에 있어 차이를 보인다. POST는 idempotent하지 않다.  

### 🚀POST
리소스 생성에 사용되는 HTTP 메서드.  
반복해서 요청할 경우 서로 다른 id를 가진 리소스를 생성한다. 즉, **idempotent 하지 않다**. 

### 🚀PUT
특정 리소스에 대한 수정을 요청할 때 사용되는 HTTP 메서드.  
반복해서 요청을 하더라도 동일한 결과를 보장한다. 즉, **idempotent** 하다.  

### 🚀2.1. 참조
- [http://1ambda.github.io/javascripts/rest-api-put-vs-post/](http://1ambda.github.io/javascripts/rest-api-put-vs-post/)

