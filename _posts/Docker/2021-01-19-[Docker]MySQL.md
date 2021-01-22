---
layout: post
title: "[Docker]MySQL Container 실행"
subtitle: "MySQL Container 실행"
categories: study
tags: Docker
---
> MySQL Container 생성 및 실행


## 컨테이너 생성 및 실행

```bash
$ docker run -p 3309:3306 --name mysql-test -e MYSQL_USER=test -e MYSQL_PASSWORD=test123 -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=root123 -d mysql
```
- `-p 3309:3306` : 로컬 포트 3309로 접속하면 컨테이너의 3306으로 접속
- `--name mysql-test` : 컨테이너명 
- MYSQL_USER : mysql 계정
- MYSQL_PASSWORD : mysql 계정 비밀번호
- MYSQL_DATABASE : 생성할 DB 명
- MYSQL_ROOT_PASSWORD : 루트 계정 비밀번호 (`필수`로 입력해야함)
- `-d` : 백그라운드 모드. detached mode.


## 컨테이너 접속
```bash
$ docker exec -it mysql-test bin/bash
root@.../# mysql -u test -p
Enter password:
```
