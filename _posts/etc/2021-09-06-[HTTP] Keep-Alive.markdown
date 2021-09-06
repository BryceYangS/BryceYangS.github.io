---
layout: post
title: "[HTTP] Keep-Alive"
subtitle: "keep alive"
categories: study
tags: etc
---
> HTTP Keep-Alive  

## Keep-Alive?

송신자가 **연결에 대한 타임아웃**과 **요청 최대 개수**를 어떻게 정했는지에 대해 알려줌  
Http는 Connectionless 방식으로 연결을 매 번 끊고 새로 생성하는 구조이다. 이는 Network 비용 측면에서 최초 연결을 하기 위해 많은 비용을 소비하는 구조.  
HTTP/1.1부터 **이미 연결되어 있는 TCP 연결을 재사용**하는 `Keep-Alive` 기능을 default로 지원.  
→ 즉, Handshake 과정이 생략되므로 성능 향상 기대  


📌`Connection`과 `Keep-Alive`는 HTTP/2에서 무시됨.  

## 문법
> Keep-Alive : *parameters*  

쉼표로 구분된 파라미터 목록으로, 각각 등호(=)로 구분되는 식별자와 값으로 구성됨  

- `timeout` : 유휴 연결이 계속 열려 있어야 하는 *최소한의 시간*(초 단위)를 가리킴. keep-alive TCP메시지가 전송 계층에 설정되지 않는다면 TCP 타임아웃 이상의 타임아웃은 무시됨.
- `max` : 연결이 닫히기 이전에 전송될 수 있는 최대 요청 수를 가리킴. 만약 `0`이 아니라면, 해당 값은 다음 응답 내에서 다른 요청이 전송될 것이므로 비-파이프라인 연결의 경우 무시됨. HTTP 파이프라인은 파이프라이닝을 제한하는 용도로 해당 값을 사용할 수 있음.


## 예제
```
HTTP/1.1 200 OK
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Thu, 11 Aug 2016 15:23:13 GMT
Keep-Alive: timeout=5, max=1000
Last-Modified: Mon, 25 Jul 2016 04:32:39 GMT
Server: Apache

(body)
```

## References
- [https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Keep-Alive](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Keep-Alive)