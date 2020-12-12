---
layout: post
title: "[Git] Git Bash 사용하기"
subtitle: "Git Bash"
categories: study
tags: Git
---

**Git Bash**
---
1. Git 이용 방법
1) Git GUI 
	- SourceTree라는 GUI 툴 사용
	- 단점 : 리눅스 지원 X, 디테일한 명령어 사용 불가
2) Git Bash
    - 윈도우에서도 리눅스 명령어 사용 가능

※ 리눅스, Git Bash 기본적인 명령어※
- 화면 초기화 : clear / Ctrl + l
- 입력한 행의 처음과 끝 : Ctrl + a, Ctrl + e
- 목록 보기 : ls 또는 dir
- 파일의 내용 보기 : cat
- 특정 문자를 검색 : grep
- 디렉터리로 이동 : cd
- 디렉터리 생성 : mkdir
- 파일 삭제 : rm
- 파일 생성 : touch

**Git Config**
---
- git config 명령어 사용
	- commit에 사용될 유저명 설정
``git config --global user.name "your_name"``
	- commit에 사용될 email 설정
``git config --global user.email "your_email@example.com"``
    - 설정 내용 확인
``git config --list``
	- username 확인
``git config user.name``

**Git 초기화 및 commit**
---
 - git은 기본적으로 새로운 파일을 관리하지 X.
 - 파일을 관리하기 위해서는 관리 대상으로 등록을 해야 함.
 - git status  : 해당 respository 내 파일들의 상태 알 수 있음
 - git add : 이 명령어를 통해 파일을 추적

**git remote**
로컬저장소와 원격저장소를 연결한다.

**Git 원격저장소와 연결한다.**
``git remote add origin [자신의 Github 또는 Bitbucket등의 원격저장소 주소]``
 
**연결된 원격저장소 확인한다.**
``remote -v``




[Reference]
 - 생활코딩(지옥에서 온 Git) : https://opentutorials.org/course/2708