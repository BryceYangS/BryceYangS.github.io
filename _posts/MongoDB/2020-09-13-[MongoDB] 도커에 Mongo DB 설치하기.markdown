---
layout: post
title: "[MongoDB] 도커에 Mongo DB 설치하기"
subtitle: "Docker에 몽고DB 설치하기"
categories: study
tags: mongo
---

# 도커에 Mongo DB 설치하기

## 몽고DB 이미지 다운로드

> docker pull mongo

## 몽고DB 컨테이너 시작

> docker run --name mongodb -p 27017:27017 -v /data/user/db:/data/db mongo

- --name : 컨테이너 이름 설정
  - 컨테이너 이름
- -p : 포트
  - <호스트 IP>:<컨테이너 IP>
  - 컨테이너의 port로 접근하기 위해서 docker의 포트 포워딩으로 연결
  - host의 27017 포트로 접근하면 컨테이너의 27017 포트로 포워딩
- -v : 볼륨
  - <호스트 디렉토리 경로>:<컨테이너 디렉토리 경로>
  - 호스트의 디렉토리를 컨테이너의 디렉토리에 마운트
  - 만약 컨테이너의 볼륨을 호스트와 연결하지 않을 경우 container 삭제 시 데이터도 같이 삭제됨

## 도커에서 몽고DB Config파일 설정 적용

- Mongod가 기본적으로 config 파일을 읽지 않기 때문에 container 시작시 confing 파일을 마운트해서 읽도록 함

> docker run -d --name mongodb -p 37017:27017 \  
> -v /home/sa/data/mongod.conf:/etc/mongod.conf \  
> -v /home/sa/data/db:/data/db mongo --config /etc/mongod.conf

## 외부 컨테이너 -> 몽고DB 컨테이너 접속

### 컨테이너 생성

- 기존 생성했던 몽고DB 컨테이너 접속을 위해 `--link` 옵션 적용

> docker run -it --name mongo-connect-test --link mongodb:mongodb mongo /bin/bash

### 외부 컨테이너에서 몽고DB 컨테이너

`$MONGO_PORT_27017_TCP_ADDR` : 연결된 컨테이너 ip  
`$MONGO_PORT_27017_TCP_PORT` : 연결된 컨테이너 port

> mongo $MONGO_PORT_27017_TCP_ADDR:$MONGO_PORT_27017_TCP_PORT

[Reference]

- stackoverflow "Docker mongodb config file": [https://stackoverflow.com/questions/37063662/docker-mongodb-config-file#comment61733751_37063727](https://stackoverflow.com/questions/37063662/docker-mongodb-config-file#comment61733751_37063727)
