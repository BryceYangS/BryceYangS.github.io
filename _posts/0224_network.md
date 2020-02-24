## 네트워크 프로토콜 TCP/IP
 - 통신하는 서버에도 TCP/IP 프로토콜이 동일해야 통신 가능
 - 그 외 네트워크 프로토콜 : Apple talk, IPX/SPX
 - 기본 설치되어있는 프로토콜 : TCP/IP

## TCP/IP 4가지 구성값
 1. IP
 2. S/M (서브넷 마스크) 할당되어있어야
 3. Gateway 값
 4. DNS 값

## LAN
 - Local Area Network
 - 하나의 회사 단위 (라우터)
	- 라우터를 통해 인터넷 망으로 연결
 - LAN 구성 
	- 1개의 라우터 ~ 다수의 스위치 ~ 컴퓨터들
 - 로컬 PC 끼리는 보통 1Gbps
 - 100GB 까지 속도가 나옴 : 상용화는 아직 X
 
## WAN
 - Wide Area Network
 - 하나의 회사 네트워크에 라우터 장비가 있다 여타 회사의 라우터 장비가 연결 되어 있음
 - LAN의 확장
 - 서비스 제공업체(ISP)에서 관리 : IANA 등의 회사 存
 - LAN보다 느림

```
> tracert www.google.co.kr
```
 - tracecert 명령어 : 내가 거쳐가는 route를 알 수 있음
 
 ## DHCP 서버
  - TCP/IP 값을 할당해줌 : IP, DNS 값
  - DHCP서버 구축 후 사용시 : ip 충돌 발생 X
  - 없을 경우 : 직접 할당해줘야함. ---> 확장성이 떨어지는 문제가 발생할 가능성 高
  - 단점 : ip가 유동적 ---> 서버는 직접 할당하는 방식으로 해야함.   
  - ip , 서브넷 마스크, Gateway, DNS 서버 4가지 값을 가져옴
  * 정리 : TCP/IP 설정 방법은 2가지가 있다. (1. 자동할당 2. 수동할당) *
  
  
  ![dhcp](/assets/img/network.png)
  
  
  네트워크
   - 속도 : 랜, HBA, 케이블, switch, Router --> 속도가 낮은 곳에 맞춰짐
   - 비용
   - 암호화 : IPsec, SSH
   - Availability : 다중화. 하나가 먹통이 되도 다른 장비가 기능할 수 있도록
   - Scalability : 확장성.
   - Reliability : 신뢰성
   - Topology : 컴퓨터 네트워크의 요소들(링크, 노드 등)을 물리적으로 연결해 놓은 것, 또는 그 연결 방식
  
  Topology
   1. Bus Topology
    - 양 끝단에 데이터를 흡수하는 터미네이터 장비 있어야.
    - 통신이 끊기면 다 끊김
   2. Star Topology
    - 일반적으로 쓰임. 구성 쉬움. 성능도 괜찮음.
   3. Ring Topology
    - token 발급 후 데이터를 token에 실어서 보냄
    - 통신이 끊기면 다 끊김
   4. Full-Mesh Topology
    - 하나의 통신이 끊겨도 큰 영향 끼치지 않음.
    
    
 - Switch : 해당 맥 주소로만 플로팅
 - Hub : 
 
 Computer -> Switch
  - Star형 구조
 Router -> Router
  - Bus형

```
> netstat -an
```
  - 로컬 주소 : 본인 컴퓨터 주소 / 외부 주소 : 현재 접속하고 있는 주소. 80포트는 현재 켜놓고 있는 웹 서버 주소.

```
cmd 실행
ncpa.cpl 입력
````
 - 랜카드, 네트워크 설정 관련   


## OSI 7 Layers
[Upper Layers]   
7. Application : 사용자와 통신   
6. Presentation : 암호화, 압축   
5. Session : 연결   

[Lower Layers]   
4. Transport : TCP / UDP  
3. Network : 목적지    
2. Data link : 스위치, 랜카드  
1.Physical : 케이블(Hub 장비)   


## CSMA/CD
 - CSMA/CD (Carrier Sense Multiple Access Collision Detect)
 - Carier Sense : 보낼 데이터가 있는 노드는 네트워크 상에 Carrier(전기신호)가 있는지 감지. 네트워크 현재 사용중인지를 확인하는 과정.
 - Multiple Sense : Carrier가 없는 것을 감지할 경우, 노드는 네트워크상에 Packet을 전송. 모든 노드가 전송 가능, 노드별 우선순위 X.
 - Collision Detection : 패킷 전송에 충돌 있는지 확인.
 - 대기 : 충돌 발생한 경우 대기 및 차례대로 전송


## Data Encapsulation   
![data encasulation](/assets/img/data_encapsulation.png)   

 - 보내는 쪽에서 Data를 잘개 쪼개 랜 카드까지 흘려 보내는 과정
 - Layer 4 Header : TCP 인지 UDP인지 값을 보내줌 ---> `Segment`라고 함
 - Layer 3 Header : 보내는 사람 ip, 받는 사람 ip ---> `Packet` : 라우터 단에서 호출 시 
 - `Frame` : 스위치 단에서 호출 시 데이터를 가리킴
 - 랜카드에서 `Bit`로 바꿈
 
 ## De-Encapsulation   
 ![de encasulation](/assets/img/de_encapsulation.png)   

  - 데이터를 받아 상단 계층으로 보내면서 header를 제거. 다시 재조립하는 과정


## Collision/Broadcast Domain  
- Collision Domain
	- Switch의 한 포트에 해당. Hub의 경우 전체가 하나의 Collision Domain
- Broadcast Domain
	- Router의 한 포트가 하나의 Broadcast Domain


- switch , Bridge : Collision Domain을 나눔
- Router : Broadcast Domain을 나눔



## The OSI Model and Nework Devices
![osi_model](/assets/img/osi_model.png)  

 - L4 switch(Advanced Switch) : Router 기능 있음, load balancing 등의 기능도 가지고 있음

## TCP/IP Protocol Stack
 - TCP/IP 먼저, OSI 7 Layer Model이 나중에 나옴  
 - OSI 7 Layer 모델 표준에 맞추면 다른 회사와 호완이 됨   


| OSI 7 Layer | TCP/IP | 
|---|---|
| Application, Presentation, session | Application | 
| Transport | Tranport | 
| Network | Network | 
| Data link, Physical | Network Interface |


Cloud는 가상 환경 기반. 가상 네트워크.


# 네트워크 장비와 실습구성

## Media
 - Media : 컴퓨터와 컴퓨터를 연결하는 케이블.
	- 주요 기능 : Bits 형태의 정보 흐름을 carry. 
 - Repeater : 신호증폭 장치
 - 100m 이상 넘어갈 경우 신호가 약해짐. Repeater를 이용해 신호 증폭.  

 - Token Ring
 - FDDI Ring
 - Ethernet Line : LAN에서 사용 케이블. Swich - Comput 연결. ( Switch - Hub - Comput 연결)
 - Serial Line : WAN에서 사용 케이블 , 라우터에서 다른 라우터로 연결할 때 사용 ( Router - Router )

## Ethernet Protocol Description
 - 예 : 100 BASE-TX
 	- 100 : LAN 속도 100Mbps
 	- BASE : Baseband(디지털) / Broad : Broadband (아날로그)
 	- TX : Cable의 물리적인 타입을 나타내거나, 숫자일 경우 최대길이 (Num * 100m)가 된다.

