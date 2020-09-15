---
layout: post
title: "[MongoDB] 도커 MongoDB 인증 추가"
subtitle: "MongoDB 인증 추가"
categories: study
tags: mongo
---

# 도커 내 Mongo DB 인증 추가하기(docker-compose 활용)

## 1. 인증 없이 몽고DB docker-compose run하기

**docker-compose.yml 파일 생성하기**

```yml
version: "3.3"
services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongodata:/data/db
    container_name: mongo
```

**Container 구동하기**

> docker-compose up -d

## 2. 몽고DB 컨테이너 접속하기

> docker exec -it mongo bash

bash로 컨테이너에 접속한 후 `mongo` 명령어로 몽고DB에 접속합니다.

## 3. 몽고DB의 유저와 권한 생성하기

먼저 admin 데이터베이스에 admin 유저를 생성합니다.

> use admin
> db.createUser({ user:"root", pwd:"root", roles:["root"] });

다음으로, 다른 유저 정보를 생성합니다.

> use demo  
> db.createUser(
> {
> user: "demo",
> pwd:"demo12345",
> roles :[
>
> {
>
> role:"readWrite",
>
> db:"demo"
>
> }
> ]
> }
> );

이제 인증을 통한 몽고DB접속을 위한 준비가 되었습니다.

## 4. 인증 접속을 위한 docker-compose 수정하기

인증 접속을 위해 `--auth` 플래그를 docker-compose파일에 추가해줍니다. 파일 수정 후 `docker-compose up -d` 명령어를 통해 컨테이너를 재구동합니다.

```yml
version: "3.3"
services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongodata:/data/db
    container_name: mongo
    command: [--auth]
    restart: always
```

## 5. 몽고DB에 접속하기

이제 인증을 통한 접속이 가능합니다.
먼저, 컨테이너에 접속을 합니다.

> docker exec -it mongo bash

컨테이너 접속 후 아래와 같은 명령어 입력을 통해 admin 데이터 베이스와 demo 데이터 베이스 각각에 접속을 할 수 있습니다.

> mongo -u root -p root --authenticationDatabase admin

> mongo -u demo -p demo12345 --authenticationDatabase demo

**참고사항**

demo 데이터베이스에 콜렉션이 하나도 없다면 접속이 되지 않는 현상이 발생했습니다. 저 같은 경우에는 `mongorestore` 명령어를 통해 해당 db를 추가해주고 나서 접속을 다시 시도하니 해결되었습니다.

[Reference]

- How to enable MongoDB authentication with docker-compose ?: [https://dev.to/efe136/how-to-enable-mongodb-authentication-with-docker-compose-2nbp](https://dev.to/efe136/how-to-enable-mongodb-authentication-with-docker-compose-2nbp)
