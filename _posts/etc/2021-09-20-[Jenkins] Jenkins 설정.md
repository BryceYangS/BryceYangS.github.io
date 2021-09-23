---
layout: post
title: "[Jenkins] Jenkins 설정"
subtitle: "Jenkins 설정"
categories: study
tags: etc
---
> Jenkin 설정

## Jenkins? 
지속적으로 통합 서비스를 제공하는 툴

### 장점
- 표준 컴파일 환경 - 빌드 테스트
- 코딩 규약 준수 여부 체크
- 테스트 환경에 대한 배포 작업
- 소스 변경에 따른 성능 변화 감시

## Jenkins 설치 
1. local : brew install jenkins / server(docker 활용) : docker pull jenkins
2. 설치 완료 후 unlock jenkins 화면이 뜸
    - 화면에 출력되는 경로로 들어가 key를 확인해서 입력
3. Customize Jenkins
    - jenkins 에서 제공하는 plugin 설치
4. Admin 계정 설정
5. Instance Configuration : jenkins url 정보 세팅


## Jenkins 관리
- JDK, Maven, Git 설정을 해줘야 함.

1. `Global Tool Configuration` 클릭
2. JDK 설정
    - JDK > Add JDK
    - oracle 계정 정보 등록
3. Git
    - `which git` 통해 경로 확인 → Path to Git executable 에 해당 경로 입력
    - 좌측 메뉴 바의 "새로운 item" 클릭
        - item name (프로젝트명) 입력 >  Freestyle project 선택
        - 구성 클릭
        - 소스 코드 관리 > Git 선택 > repository URL 입력
            - Credintials 의 Add 클릭, Jenkins 선택 → git 계정 정보 등록
        - Build
            - Invoke top-level Maven targets 선택
            - Goals : package 입력
            - 고급 클릭
                - POM : pom.xml 경로 입력

- Build 폴더 권한을 줘야 함
    - build now 를 해서 생성된 폴더에 권한을 줌
    - `chmod 777`을 통해 모든 권한을 줌 → Jenkins 관리의 Reload Configuration from Disk 클릭


## 배포 및 파이프라인 구축
Jenkins 관리 > 설치 가능 탭 > `Publish Over SSH` 선택 후 플러그인 설치  


