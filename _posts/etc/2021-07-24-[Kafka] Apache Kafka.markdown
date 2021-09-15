---
layout: post
title: "[Kafka] Apache Kafka"
subtitle: "kafka"
categories: study
tags: etc
---
# 🐳Apache Kafka

## 💫아파치 카프카 개요 및 설명

* 흐름
    * Source Application → KafKa → Target Application
* 대용량 데이터 처리 용이
* 어플리케이션 데이터 간의 종속성을 약화

## 💫토픽이란?

* 데이터가 들어가는 공간
* 토픽 여러 개 생성 가능.
* DB 테이블 또는 파일 시스템의 폴더와 유사
* 목적에 따라 토픽을 만들면 관리 용이

## 💫토픽 내부
![topic](/assets/img/etc/kafka/kafka1.png)

* 하나의 토픽은 여러개의 파티션을 가질 수 있음
* 파티션
    * 0부터 시작
    * 파티션 내에 데이터가 쌓임. 각 데이터는 오프셋(offset)이라고 하는 숫자가 붙게됨.
    * 컨슈머는 파티션의 가장 오래된 데이터부터 가져감
    * 컨슈머가 record들을 가져가도 데이터는 삭제되지 않음
        * 지워지지 않은 데이터는 새로운 컨슈머가 다시 0부터 데이터 가져감
            * 단, 컨슈머 그룹이 달라야 하고, auto.offset.reset = earliset 세팅되어 있어야 함.
    * 파티션 2개 이상 경우
        * 프로듀서가 키 설정 가능
            1. 키 null & 기본 파티셔너 사용 경우
                * Round Robin 으로 할당
            2. 키 not null & 기본 파티셔너 사용할 경우
                * 키의 hash 값을 구해 특정 파티션에 할당
        * 파티션 늘릴 때 유의!!!
            * 파티션을 늘릴 수 있지만 줄일 수는 없음
            * Why 파티션 증가? 파티션을 늘리면 데이터 처리 분산 시킬 수 있음
    * Record 언제 삭제?
        * 옵션에 따라 다름
            * log.retention.ms : 최대 record 보존 시간
            * log.retention.byte : 최대 record 보존 크기(byte)

## 💫브로커, 복제, ISR

### 🐬카프카 브로커
* 카프카가 설치되어 있는 서버 단위
* 보통 3개 이상의 broker 권장
* 브로커 개수가 3이면 replication은 3을 초과할 수 없음

### 🐬Replication
* 파티션 복제를 의미
* Eg) replication=1 : partition 1개만 존재 / 
    * replication=3 : partition 원본 1개 / 복제본 2개
* Leader partition : 데이터 원본을 저장하는 파티션
    * 프로듀서가 토픽의 파티션에 데이터 전달 시 전달받는 주체
    * 프로듀서의 ack 옵션 : partition의 replication과 관련
        * 0 : Leader partition에 데이터 전송 후 응답값 받지 않음
            * 속도 빠르나 / 데이터 유실 가능성 존재
        * 1 : leader partition에 데이터 전송 후 정상 응답 여부 응답값 전달
            * 복제 여부 확인 불가 : 데이터 유실 가능성 존재. (Leader 데이터 전달 받은 즉시 바로 문제 발생 시 복제가 정상 작동하지 않을 수 있음)
        * all : 응답 + 복제 정상 여부
            * 데이터 유실은 없으나 / 속도 현저히 느림
* Follower partition : 원본 저장 제외 파티션
* ISR (In Sync Replica) : Leader + Follower partitions
* 고가용성 목적으로 Replication 활용
    * 하나의 파티션에서 문제 발생 시 다른 파티션 활용 가능
* 많으면 좋은게 아님
    * 브로커가 사용하는 리소스 양도 증가 (eg. Disk usage 1TB)
    * 데이터량 & 저장시간 고려한 레플리케이션 개수 결정
    * 브로커 3개이상 사용시 레플리케이션 3 추천


## 💫파티셔너란?
￼![partitioner](/assets/img/etc/kafka/kafka2.png)
* 프로듀서는 파티셔너를 통해 데이터를 브로커로 전달
* 데이터를 토픽의 어떤 파티션에 넣을지 결정하는 역할
    * 레코드에 포함된 메시지 키 또는 메시지 값에 따라 파티션의 위치 결정

* 디폴트 파티셔너 : UniformStickyPartitioner
    * 메시지 키 유무에 따라 다르게 작동
        1. 메시지 키 존재 시
            * 해시 값 => 파티션 번호
            * 해시 값이 동일할 경우 동일한 파티션에 순서대로 저장되는 것을 보장할 수 있음.
        2. 메시지 키 없을 시
            * Round Robin으로 저장
                * 조금 다르게 저장. 프로듀서에서 배치로 모을 수 있는 데이터를 모아 파티션에 보냄
            * 파티션에 적절히 분배됨
* Custom Partitioner 사용 가능
    * Kafka에서 Partitioner 인터페이스 제공
    * 언제 사용?
        * Eg. vip고객을 위해 데이터 처리를 조금 더 빨리 처리. 처리량을 늘릴 수 있음. Vip 고객을 위한 파티션 개수를 더 많이 가져감.


## 💫Kafka Lag이란?
카프카 모니터링 지표.

### 🐬Kafka Consumer Lag
* 프로듀서가 마지막으로 넣은 offset과 컨슈머가 마지막으로 읽은 offset 간의 차이
* 토픽 내 파티션 여러 개 존재 시 lag 역시 여러 개 존재 가능
* 주로 컨슈머의 상태에 대해 볼 때 사용
* Records-lag-max
    * 한 개의 토픽과 컨슈머 그룹에 대한 lag이 여러개 존재할 수 있을 때 그 중 가장 높은 숫자의 lag

## 💫Kafka Burrow
- KafkaConsumer 객체를 통해 현재 Lag 정보 가져올 수 있음. Lag 모니터링을 컨슈머 단위에서 하는 것은 위험하고 운영요소가 많이 들어감
    - Why? 
        - 컨슈머 상태에 디펜던시가 걸리기 때문. 컨슈머가 비정상적으로 종료될 경우 더이상 lag 정보 보낼 수 없음. Lag 측정 불가해짐.
        - Consumer 추가될 때마다 Lag 측정 기능 개발 필요.
- 오픈소스
- Golang
- 컨슈머 Lag 모니터링을 도와주는 독립적인 애플리케이션
- 3가지 특징
    1. 멀티 카프카 클러스터 지원
        - 카프카 클러스터가 여러 개더라도 버로우 1개에만 연동하면 카프카 클러스터들에 붙은 컨슈머의 Lag를 모두 모니터링 가능
    2. Sliding window를 통한 Consumer의 status 확인
        - ‘ERROR’, ‘WARNING’, ‘OK’ 
            - ERROR : Consumer가 데이터를 가져가지 않는 경우
            - WARNING : 데이터양이 일시적으로 많아지면서 Consumer offset이 증가되고 있는 경우
            - OK
    3. HTTP API 제공

Burrow 소개 : [https://blog.voidmainvoid.net/243](https://blog.voidmainvoid.net/243)  
Burrow의 Consumer status 확인 방법 : [https://blog.voidmainvoid.net/244](https://blog.voidmainvoid.net/244)  
Burrow http endpoint 정리 : [https://blog.voidmainvoid.net/245](https://blog.voidmainvoid.net/245)  
Burrow github : [https://github.com/linkedin/Burrow](https://github.com/linkedin/Burrow)  


AWS에 카프카 클러스터 설치하기 : https://blog.voidmainvoid.net/325


## 💫Kafka Producer

### 🐬프로듀서 역할
- Topic에 해당하는 메시지 생성
- 특정 topic으로 데이터 publish
- 처리 실패/재시도

Kafka-client 라이브러리 의존성 추가 필요.
- 브로커 버전 & 클라이언트 버전 호환성 확인 필수!!!

### 🐬Kafka Producer 적용 Java 소스
![producer](/assets/img/etc/kafka/kafka3.png)
￼

### 🐬Key가 Null 인 경우
![producer key null](/assets/img/etc/kafka/kafka4.png)￼

### 🐬Key를 지정한 경우
- **파티션을 중간에 추가하는 경우 key와 파티션의 매칭이 깨짐!!**
    - **중간에 추가적인 파티션을 생성하지 않는 것을 권장함.**


￼![producer key not null](/assets/img/etc/kafka/kafka5.png)￼


## 💫Kafka Consumer
파티션에 저장된 데이터 가져옴(polling)

### 🐬컨슈머 역할
- Topic의 partition으로부터 데이터 Polling
- Partition offset 위치 기록 (commit)
- Consumer group을 통해 병렬 처리

Kafka-client 라이브러리 의존성 추가 필요.
- 브로커 버전 & 클라이언트 버전 호환성 확인 필수!!!

다른 메시징 시스템과 다른 점
- 컨슈머가 데이터를 가져가면 큐 내부 데이터 사라짐
- 카프카는 큐 내부 데이터 사라지지 않음

### 🐬Kafka Consumer 적용 Java 소스
￼￼![consumer](/assets/img/etc/kafka/kafka6.png)￼
 
- 폴링 루프 : poll() 메서드가 포함된 무한루프
    - Consumer API 핵심 로직
    - 0.5초 동안 데이터 도착 기다림. 이후 코드 실행
    - 만약 0.5초동안 데이터가 들어오지 않으면 빈값을 레코드 반환 / 데이터 존재 시 records 값 반환
    - records는 데이터 배치로, 레코드의 묶음 list   


￼￼![consumer](/assets/img/etc/kafka/kafka7.png)￼

### 🐬Kafka Consumer 작동방식
￼￼￼![consumer](/assets/img/etc/kafka/kafka8.png)￼

- offset은 토픽별, 파티션별로 메겨짐
    - offset은 컨슈머가 데이터를 어느지점까지 읽었는지 확인하는 용도
- 컨슈머가 데이터를 가져갈때마다 카프카의 __consumer_offset 토픽에 offset 정보를 저장 (commit)
    - 예기치 않게 실행이 중단될 경우, __consumer_offset 토픽의 offset 정보를 토대로 시작위치 복구 가능


### 🐬동일한 Kafka Consumer Group
￼￼![consumer](/assets/img/etc/kafka/kafka9.png)￼
￼

### 🐬여러 개의 Kafka Consumer Group
￼￼￼![consumer](/assets/img/etc/kafka/kafka10.png)￼

- 서로 다른 컨슈머 그룹에 대한 offset 정보는 영향을 미치지 않음
- __consumer_offset 토픽에 컨슈머 그룹별로 토픽별로 offset을 나누어 저장하기 때문


## 참조
[\[데브원영\]아파치 카프카 for beginners](https://www.inflearn.com/course/아파치-카프카-입문/dashboard)