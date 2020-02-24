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
  
