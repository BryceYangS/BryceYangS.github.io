---
layout: post
title: "[Database] Sharding & Replication & Clustering"
subtitle: "sharding, replication"
categories: study
tags: db
---
> DB Sharding, Replication, Clustering

### 🚀Partitioning
**파티셔닝**은 일반적으로 DB 테이블을 작은 부분으로 여러 개 나누는 것을 가리킴.  
Column을 기준으로 나누는 `Vertical Partitioning`(수직)과 Row를 기준으로 나누는 `Horizontal Partioning`(수평)이 있다.

### 🚀Sharding
**샤딩**은 `수평 파티셔닝`의 특수한 형태. `Shard key`라고 알려진 키를 기준으로 데이터가 나뉨. Shard key 알고리즘에 따라 여러 Sharding이 있음.  
샤딩은 여러 서버에 스키마가 복제됨. 데이터는 Shard key를 기준으로 여러 노드들에 나누어 저장됨.  
검색 요청에 대한 부하를 서로 다른 샤드가 있는 여러 서버에 분산할 수 있음.  
스키마를 유지하기 때문에 *DB 확장에 있어서도 적용이 쉬움*.  

### 🚀Replication
> 여러 개의 DB를 수직적인 구조(Master - Slave)로 구축하는 방식  

- Master Node는 `쓰기` 작업만을 처리, Slave Node는 `읽기` 작업만을 처리.  
- **비동기 방식**으로 노드들 간의 데이터 동기화
- 장점
    - DB 요청의 대부분이 Read이기 때문에 **성능** 상 이점
    - 비동기 방식으로 운영되어 지연 시간이 거의 없음
- 단점
    - 노드들 간의 데이터 동기화가 보장되지 않아 **일관성있는 데이터를 얻지 못할 수** 있다.
    - Master 노드가 다운되면 복구 및 대처가 까다로움

#### 💻MySQL의 Replication

![mysql-replication](/assets/img/database/mysql_replication.png)

1. Master 노드에 쓰기 트랜잭션이 수행됨
2. Master 노드는 데이터를 저장하고 트랜잭션에 대한 로그를 파일에 기록(BIN LOG)
3. Slave 노드의 IO Thread는 Master 노드의 로그 파일(BIN LOG)를 파일(Replay Log)에 복사한다.
4. Slave 노드의 SQL Thread는 파일(Replay Log)를 한 줄씩 읽으며 데이터를 저장한다.

### 🚀Clustering
> 여러 개의 DB를 수평적인 구조로 구축하는 방식  

- 클러스터링은 분산 환경을 구성해 Single point of failure와 같은 문제를 해결할 수 있는 Fail Over 시스템을 구축하기 위해서 사용됨
- **동기 방식**으로 노드들 간의 데이터를 동기화


### 🚀4.2. 참조
- [https://www.youtube.com/watch?v=AxpsOLLQt3o](https://www.youtube.com/watch?v=AxpsOLLQt3o)
- [https://mangkyu.tistory.com/97](https://mangkyu.tistory.com/97)
