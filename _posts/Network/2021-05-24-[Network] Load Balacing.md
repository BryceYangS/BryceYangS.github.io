---
layout: post
title: "[Network] Load Balacing"
subtitle: "Load Balacing"
categories: study
tags: network
---

> Load Balacing

## Load Balancing
> 여러 대의 서버가 분산 처리할 수 있도록 요청을 나누어주는 서비스  

클라이언트가 증가해 응답해야 하는 서버에 과부하가 걸리는 현상이 발생. 서버가 멈추는 경우가 발생하면 안되기 때문에 서버 증가가 필요하게 됨.  

### 🚀서버 성능 증가 방법
1. **Scale-Up** : 서버가 더 빠르게 동작하기 위해 ***하드웨어 성능***을 올림
2. **Scale-Out** : ***서버의 개수***를 늘려 여러 서버가 요청을 나눠서 처리

### 🚀Load Balancer 종류
OSI 7 Layer 기준으로 어떤 것을 나누는지에 따라 다름  

#### 💻 1. L2
Mac 주소 기준 Load Balancing  

#### 💻 2. L3
IP 주소 기준  

#### 💻 3. L4
Transport Layer (IP & Port) 레벨에서 Load Balancing  
IP와 Port 정보를 보고 정해진 정책에 따라 라우팅  
ie) 서버 A, 서버 B로 로드 밸런싱  

#### 💻 4. L7
Application Layer(User Request) 레벨에서 Load Balancing (HTTPS, HTTP, FTP)  
IP와 Port정보 뿐만 아니라 패킷의 URL 정보, 쿠키, payload 정보들을 보고 정해진 정책에 따라 라우팅  
하위 로드밸런서보다 세부적인 로드 밸런싱이 가능  
마이크로서비스 간의 통신은 보통 HTTP 사용. 그래서 HTTP 기반 로드 밸런서는 이러한 통신을 쉽게 처리할 수 있다는 장점도 있음  
하지만, 패킷 내용을 복호화하여 처리를 해야하기 때문에 부하가 많이 걸릴 수 있음.  

### 🚀L4 와 L7 로드 밸런싱의 차이점
**L4 로드 밸런싱**은 메시지 내용에 관계없이 메시지 전달을 처리.  
L4 Load Balancer는 패킷의 내용을 검사하지 않고 단순히 업스트림 서버와 네트워크 패킷을 전달함.

**L7 로드 밸런싱**은 각 메시지의 실제 내용을 처리하는 상위 수준 애플리케이션 레이어에서 작동.  
L7 로드 밸런서는 L4 로드 밸런서보다 훨씬 정교한 방식으로 네트워크 트래픽을 라우팅하며, 특히 HTTP와 같은 TCP 기반 트래픽에 적용됨.   
L7 로드 밸런서는 네트워크 트래픽을 종료하고 그 안에서 메시지를 읽음. 메시지 내용(ie: URL 또는 쿠키)을 기반으로 부하 분산 결정을 내릴 수 있음. 그런 다음 선택한 업스트림 서버에 새 TCP 연결을 만들고 서버에 요청.  
L7 로드 밸런싱은 동일한 서버가 복제되는 것이 아닌, 서로 다른 역할을 가진 서버로 라우팅을 하는 것. 예는 다음과 같음.
- 서버 1 : 이미지와 그래픽을 제공
- 서버 2 : CSS, HTML과 같은 스크립팅 및 콘텐츠를 사용해 사이트 방문자에게 컨텐츠 제공
- 서버 3 : 사용자에게 컨텐츠 구매 처리 기능 제공
- 서버 4 : 구매한 컨텐츠 제공

L7 로드 밸런싱을 사용하면 *실제 리소스 사용량*을 기반으로 훨씬 더 복잡한 애플리케이션 및 컨텐츠 전달 모델을 사용할 수 있음.

### 🚀3.1. 참조
- [10분 테코톡] 🐿 제이미의 Forward Proxy, Reverse Proxy, Load Balancer : [https://www.youtube.com/watch?v=YxwYhenZ3BE](https://www.youtube.com/watch?v=YxwYhenZ3BE)
- [https://peemangit.tistory.com/197](https://peemangit.tistory.com/197)
- [https://gruuuuu.github.io/network/lb01/](https://gruuuuu.github.io/network/lb01/)
- [https://www.nginx.com/resources/glossary/layer-7-load-balancing/](https://www.nginx.com/resources/glossary/layer-7-load-balancing/)
- [https://kemptechnologies.com/blog/what-the-heck-is-layer-7-load-balancing-anyway/](https://kemptechnologies.com/blog/what-the-heck-is-layer-7-load-balancing-anyway/)