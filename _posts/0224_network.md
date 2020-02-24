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

### Data Encapsulation   
![data encasulation](/assets/img/data_encapsulation.png)   

 - 보내는 쪽에서 Data를 잘개 쪼개 랜 카드까지 흘려 보내는 과정
 - Layer 4 Header : TCP 인지 UDP인지 값을 보내줌 ---> `Segment`라고 함
 - Layer 3 Header : 보내는 사람 ip, 받는 사람 ip ---> `Packet` : 라우터 단에서 호출 시 
 - `Frame` : 스위치 단에서 호출 시
 - 랜카드에서 Bit로 바꿈
 
 ## De-Encapsulation   
 ![de encasulation](/assets/img/de_encapsulation.png)   

  - 데이터를 받아 상단 계층으로 보내면서 header를 제거. 다시 재조립하는 과정
