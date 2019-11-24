---
layout: post
title: "[Docker] Docker?"
subtitle: "Docker?"
categories: study
tags: Docker
---

**Docker란 무엇인가**
---
1. Docker 정의
 - 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는 소프트웨어 플랫폼
 - 소프트웨어를 컨테이너라는 표준화된 유닛으로 패키징
 - 컨테이너 내 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어 실행 환경이 포함되어 있음
 - Kubernetes는 컨테이너 애플리케이션의 배포, 확장 및 관리를 자동화 하는 오픈 소스
2. Docker 작동 방식
	![Container&VM](https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_2-VM-vs-Containers.78f841efba175556d82f64d1779eb8b725de398d.png)
- VM은 서버 하드웨어를 가상화하는 방식 vs Container 기반의 Docker는 서버 운영 체제를 가상화
- Docker는 각 서버에 설치되며 컨테이너를 구축, 시작 또는 중단하는 데 사용할 수 있는 명령 제공 
3. Docker Architecture
- Container VS VM
	- Docker 플랫폼은 하드웨어 영역에서 OS 단계로 소스를 추상화
	- VM은 전체 하드웨어 서버를 추상화
	- Container는 OS kernel을 추상화
		- 가상화의 전혀 다른 접근법, 더 가벼운 instance

**Docker Engine**
---
- Docker Engine은 다음과 같은 구성요소로 애플리케이션 개발, run.

**1) Docker Daemon**
 -- Docker image, container, network, storage volume을 관리하는 백그라운드 프로세스
 -- 끊임없이 Docker API 요청을 받고 처리함 
**2) Docker Engine REST API**
--  애플리케이션이 Docker Daemon과 상호작용하기 위한 API
-- HTTP 클라이언트에 의해 접근
**3) Docker CLI**
-- Docker 대몬과 상호 작용하기 위한 명령줄 인터페이스 클라이언트.
![Docker Engine](https://www.aquasec.com/wiki/download/attachments/2854889/Docker_Engine.png?version=1&modificationDate=1520172702424&api=v2)



[Reference]
 - AWS Docker : https://aws.amazon.com/ko/docker/
 - Docker Engine : https://www.aquasec.com/wiki/display/containers/Docker+Architecture#DockerArchitecture-TheDockerEngine