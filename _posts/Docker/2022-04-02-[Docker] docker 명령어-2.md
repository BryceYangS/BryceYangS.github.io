---
layout: post
title: "[Docker] docker 명령어-2"
subtitle: "Docker 명령어"
categories: study
tags: Docker
---
> Docker 명령어

## 도커 컨테이너
- docker start CONTAINER
- docker attach CONTAINER : 내부로 들어감
- docker ps : 컨테이너 조회 (-a 옵션 : 정지 컨테이너 포함 전체 조회)
- docker rm CONTAINER : 컨테이너 삭제
- docker container prune : 컨테이너 일괄 삭제
- docker run  
  - -p 옵션 : \[호스트의 포트]:[컨테이너의 포트]
    - `docker run -i -t -p 3306:3306 -p 192.168.0.100:7777:80 ubuntu`
  - -v 옵션 : 호스트 볼륨 공유 : \[호스트의 공유 디렉터리]:[컨테이너의 공유 디렉터리]

## 도커 볼륨(docker volume)
- 도커 볼륨은 여러 개의 컨테이너에 공유되어 활용 가능
- docker volume create --name myvolume
- docker volume ls
- docker run -i -t --name myvolume_1 **-v myvolume:/root/** ubuntu
  - \[볼륨 명]:[컨테이너의 공유 디렉터리]
- docker inspect --type volume myvolume
- docker run -i -t --name volume_auto -v /root ubuntu
  - 볼륨 자동 생성

## 도커 네트워크
- docker network ls
- docker network create --driver bridge mybridge
  - docker run -i -t --name mynetwork_container **--net mybridge** ubuntu
- docker network disconnect/connect NETWORK CONTAINER : 네트워크 끊기/연결
- docker port CONTAINER : 포트 확인

## 도커 로그
- docker logs -f -t
  - -f : 로그 스트림으로 확인
  - -t : 타임스탬프


## 도커 이미지
- docker images : 도커 이미지 조회
  - --filter "label=key=value" : 이미지 조회 옵션
- docker commit \[OPTIONS] CONTAINER \[REPOSITORY[:TAG]] : 이미지 생성
- docker history IMAGE : 이미지 레이어 구성 조회
- docker save : 컨테이너의 커맨드, 이미지 이름과 태그 등 이미지의 모든 메타데이터 포함 하나의 파일로 추출
  - docker save -o ubuntu_14_04.tar ubuntu:14.04
- docker load : 이미지 로드
  - docker load -i ubuntu_14_04.tar
- docker export : 컨테이너의 파일시스템을 tar 파일로 추출. 컨테이너 및 이미지에 대한 설정 정보 저장 X
  - docker export -o rootFS.tar mycontainer
  - docker import rootFS.tar myimage:0.0