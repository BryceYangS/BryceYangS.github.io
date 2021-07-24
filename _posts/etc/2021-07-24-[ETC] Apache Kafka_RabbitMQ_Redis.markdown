---
layout: post
title: "[ETC] Kafka vs Rabbit MQ vs Redis"
subtitle: "Kafka vs Rabbit MQ vs Redis"
categories: study
tags: etc
---
# 🍭Kafka vs Rabbit MQ vs Redis

## 🍬메시지 브로커 vs 이벤트 브로커
### 🍫메시지 브로커
* 이벤트 브로커 역할 불가
* 미들웨어 아키텍처
* 메시지를 받아서 적절히 처리하고 나면 즉시 또는 짧은 시간 내에 삭제되는 구조	
* 데이터를 보내고 처리하고 삭제
* Redis, Rabbit MQ

### 🍫이벤트 브로커
* 메시지 브로커 역할 가능
* 이벤트 또는 메시지 라고 불리는 레코드를 딱 하나만 보관, 인덱스를 통해 개별 액세스를 관리
* 업무상 필요한 시간동안 이벤트를 보존할 수 있음
* 데이터 삭제 X
* 장점
    1. 딱 한 번 일어난 이벤트 데이터를 브로커에 저장함으로써 단일 진실 공급원으로 사용 가능
    2. 장애가 발생했을 시 장애가 일어난 지점부터 재처리 가능
    3. 많은 양의 실시간 스트림 데이터를 효과적으로 처리 가능
* Kafka, AWS의 키네시스


## 참조
[https://www.youtube.com/watch?v=H_DaPyUOeTo](https://www.youtube.com/watch?v=H_DaPyUOeTo)