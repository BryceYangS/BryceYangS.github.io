---
layout: post
title: "[Data] Apache Pinot Cluster 구축"
subtitle: "apache pinot"
categories: study
tags: etc
---
> Apache Pinot Cluster 구축

# Apache Pinot Cluster 구축 따라해보기
**클러스터 구성**  
- Zookeeper : 1
- Controller : 2
- Broker : 2
- Server : 2


## Pinot Components
**Zookeeper**
- `메타데이터 저장소`

**Pinot Controller**
- Controller는 Apache Helix를 호스팅하며, `클러스터 관리` 기능 제공



## Docker로 띄워보기

```script
docker run \
    -p 2123:2123 \
    -p 9000:9000 \
    -p 8000:8000 \
    -p 7050:7050 \
    -p 6000:6000 \
    apachepinot/pinot:1.1.0 QuickStart \
    -type batch
```  

- 2123: Zookeeper Port
- 9000: Pinot Controller Port
- 8000: Pinot Broker Port
- 7050: Pinot Server Port
- 6000: Pinot Minino Port



### [References]
- [How to setup a Pinot cluster Youtube](https://www.youtube.com/watch?v=cNnwMF0pOJ8&t=15s)
