---
layout: post
title: "[etc]Nginx"
subtitle: "Nginx"
categories: study
tags: etc
---


## 1. Nginx
- Apache HTTPd 취약점 보완하기 위해 개발
- Apache C10K 문제 : 10,000개 이상의 소켓을 열게 되면 하드웨어 성능이 충분함에도 불구하고 I/O 처리 방식의 문제 때문에 프로세스가 제대로 처리하지 못함
- Event-Driven 구조로 만든 웹 서버
- 어플리케이션 서버가 동작하고 그 하위 레벨에서 Nginx같은 웹서버가 HTTP 통신을 제공

## 2.Apache vs Nginx
### 2.1 Apache
- Client가 HTTP 요청을 보낼 때, MPM(Multi-Process Module)을 사용해 처리.
- 쓰레드 : 클라이언트 --> 1 : 1구조

## MPM 두 가지 방식

**1. PreFork (다중 프로세스 처리)**

- Client 요청에 대해 default 개수만큼 apache 자식 프로세스를 생성해 처리.
- 요청이 많을 경우 프로세스를 생성해 처리하는 방식.
- Apache 설치시 default로 설정되어 있음.

**2. Worker (멀티 프로스세스 - 스레드)**

- Prefork와 같이 default Apache 자식 프로세스를 생성하고 요청이 많아지면 각 프로세스의 스레드를 생성해 처리.

### 2.2 Nginx

- Event-Driven 방식 동작.
- 한 개 또는 고정된 프로세스만 생성, 그 프로세스 내부에서 `비동기 방식`으로 처리 - 접속 요청이 많아도 프로세스 또는 쓰레드 생성 비용이 발생 X  
![event-driven](/assets/img/etc/event-driven.png)


```
여러개의 커넥션을 Event Handler를 통해 비동기 방식으로 처리해 먼저 처리되는 것부터 로직이 진행됨.
```

## Reverse Proxy?
### 1) Forward Proxy
- 여러 클라이언트가 인터넷에 요청을 할 때 그 중간에서 Proxy 서버를 두는 경우
- 클라이언트 보호 목적

### 2) Reverse Proxy
- 인터넷 상의 여러 컴퓨터(클라이언트)가 서버에 요청을 할 때 그 중간에 Proxy 서버를 두는 경우
- 서버 보안 강화 목적 : 서버 IP를 숨겨줌
- 기능
  - 트래픽 제어 (eg. DDOS 공격으로부터 보호 가능)
  - 로드 밸런싱


## Nginx
- 서버의 앞단의 리버스 프록시로써 로드 밸런싱, 암호화(HTTPS의 encrytion), 캐싱, 트래픽 제어 등의 기능을 함.
- nginx는 다양한 변수를 제공 ([변수 문서](https://nginx.org/en/docs/varindex.html))

## 참조

- [큰돌의 터전](https://m.blog.naver.com/jhc9639/220967352282)
- [Proxy vs Reverse Proxy Youtube](https://www.youtube.com/watch?v=RXXRguaHZs0)
- [NGINX Tutorial for Beginners Youtube](https://www.youtube.com/watch?v=9t9Mp0BGnyI)