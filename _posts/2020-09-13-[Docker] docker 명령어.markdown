---
layout: post
title: "[Docker] docker 명령어"
subtitle: "Docker 명령어"
categories: study
tags: Docker
---

# Docker 명령어

## 도커 이미지

### 이미지 가져오기

> docker pull [이미지명]:[태그]

### 이미지 목록

> docker images

## 도커 컨테이너

### 이미지로 컨테이너 생성

> docker create [옵션] [이미지명]:[태그]

### 컨테이너 실행

> docker start [이미지명]:[태그]

### 컨테이너 내부 접속

> docker attach [컨테이너명]

- 외부에서 컨테이너에 명령
  - `-it` 옵션 : 컨테이너 shell 접속

> docker exec -i -t [컨테이너명] bash

### 컨테이너 생성 및 실행

`생성` -> `실행` -> `접속`

> docker run [옵션] [이미지명]:[태그]

`-i` : 컨테이너와 상호작용을 하겠다는 뜻이고 컨테이너와 표준 입력을 유지합니다.  
대부분 이 옵션을 사용하여 bash 명령어를 인자로 같이 사용합니다.  
`-t` : TTY 모드를 사용합니다. Bash를 사용하려면 이 옵션과 같이 사용해야 합니다.  
`--name` : 컨테이너 이름 지정
`-d` : 백그라운드로 실행  
`-e` : 컨테이너에 환경 변수를 설정합니다. 비밀번호나 설정 값을 전달할 수 있습니다.  
`-p` : 호스트에 연결된 컨테이너의 특정 포트를 외부에 노출하게 됩니다.

### 컨테이너 목록

`-a` 옵션 : 종료된 컨테이너 포함. 전체 컨테이너 목록.
`-q` 옵션 : 컨테이너 ID만 출력

> docker ps

### 컨테이너 삭제

> docker rm [컨테이너명]

- 전체 컨테이너 삭제

> docker container prune

### 컨테이너 ip 확인

> docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [컨테이너명]
