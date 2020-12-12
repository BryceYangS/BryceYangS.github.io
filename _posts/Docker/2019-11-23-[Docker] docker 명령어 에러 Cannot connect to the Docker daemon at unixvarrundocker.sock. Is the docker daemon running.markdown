---
layout: post
title: "[Docker] docker 명령어 에러 Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running"
subtitle: "Docker 명령어 에러"
categories: study
tags: Docker
---

## Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running
-------------
 - 도커 에러 로그가 위와 같을 경우 도커가 실행되지 않은 것
 - **$sudo systemctl status docker** 
	- docker 서비스 상태 확인 명령
	- Active 값이 dead 혹은 stop 일 시 아래 명령어 실행
 - **$sudo systemctl start docker** 
 - **$sudo systemctl enable docker**
 
 ----> Active 되었는지 확인!