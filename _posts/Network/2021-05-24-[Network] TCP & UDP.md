---
layout: post
title: "[Network] TCP & UDP"
subtitle: "TCP & UDP"
categories: study
tags: network
---

> TCP & UDP 개념

### 🚀TCP
전송 제어 프로토콜 (Transmission Control Protocol)  
1. 연결지향 - TCP 3 way handshake
    1) Client - SYN : 접속 요청
    2) Server - SYN + ACK : 요청 수락
    3) Client - ACK : ACK와 함께 데이터 전송도 가능. 즉, 4단계 생략 가능
    4) Client - 데이터 전송
2. 데이터 전달 보증
3. 순서 보장

#### 💻TCP 신뢰성을 유지할 수 있는 방법
##### 1. 잘 받았으면 ACK, 못 받았으면 NAK
오류가 날 경우 보냈던 TCP Segment를 다시 보내주게 된다. 이를 위해 수신자는 잘 받았다면 **ACK(Positive Acknowledge)**를, 중간에 오류가 났다면 **NAK(Negative Acknowledge)**를 송신자에게 보내주게 된다. 이를 통해 송신자는 다시 TCP Segment를 보낼지 말지 결정하게 된다.  
TCP Segment의 `Header` 부분을 통해서 오류를 확인할 수 있고, 순서가 바뀐 경우를 확인할 수 있다.

![tcp-header](/assets/img/network/tcp_header.png)

##### 2. 어떻게 수신자는 TCP Segment에 오류가 있는지 알 수 있는가?
TCP Header에서 128비트 부터 시작하는 `Checksum` 부분에서 오류를 확인할 수 있음. Checksum Error Detecting을 통해 수신자는 송신자가 보낸 데이터가 제대로 보내졌는지 확인할 수 있으며 잘 못 보내졌을 경우 TCP Flag(NS, CWR, ECE, URG, ACK, PSH, RST, SYN, FIN) 중에서 ACK flag를 `reset(0)`하여 보낸다. 만일 잘 보내졌을 경우 ACK flage를 `set(1)`하고 Acknowlegement number에 수신자가 받았던 sequence number에 1을 더한 값을 넣어 보내준다. 이렇게 해야 순서가 섞여있는 TCP 프로토콜에서 제대로 통신할 수 있다.

##### 3. 순서가 뒤바뀐 TCP Segment는 어떻게 처리?
`Sequence number`가 있기 때문에 수신자 측에서 Sequence number 순서대로 데이터 청크들(data chunks)을 잘 붙여주기만 하면 되기 때문이다.

##### 4. 만약 수신자가 송신자에게 ACK, NCK도 못 보낼 상황인 경우
`timer`, `timeout`개념을 이용. 일정 시간 동안 ACK 또는 NCK가 오지 않는 다면 timeout된 시점에서 다시 TCP Segment를 보내주게 된다. timer 설정에는 다양한 알고리즘이 적용되며, 대표적으로 Kan's algorithm이 있다.

### 🚀UDP
사용자 데이터그램 프로토콜(User Datagram Protocol)  
- 연결 지향 - TCP 3 way handshake❌
- 데이터 전달 보증 ❌
- 순서 보장 ❌
- 단순하고 빠름
- IP 통신과 거의 같음 + 알파 : Port, 체크섬 기능 정도만 추가됨

### 🚀3.2. 참조
- [https://wjdtn7823.tistory.com/37](https://wjdtn7823.tistory.com/37)
- 인프런, 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC#)
- [https://velog.io/@sms8377/Network-TCP%EC%9D%98-%EC%9E%AC%EC%A0%84%EC%86%A1%EA%B3%BC-%ED%83%80%EC%9E%84-%EC%95%84%EC%9B%83](https://velog.io/@sms8377/Network-TCP%EC%9D%98-%EC%9E%AC%EC%A0%84%EC%86%A1%EA%B3%BC-%ED%83%80%EC%9E%84-%EC%95%84%EC%9B%83)