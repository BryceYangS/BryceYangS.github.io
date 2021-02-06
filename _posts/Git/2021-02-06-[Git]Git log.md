---
layout: post
title: "[Git]Git log"
subtitle: "Git log를 보자"
categories: study
tags: Git
---
> Git의 log 확인


### 1. --pretty=oneline
```git
$ git log --pretty=oneline
```
commit history를 한 줄로 볼 수 있음

pretty 값에는 oneline, short, medium, full, fuller, reference, email, raw, format:<string> and tformat:<string> 넣을 수 있음 

`git log --oneline` : --pretty=oneline --abbrev-commit 두 옵션을 합친 것과 같음  

`-abbrev-commit` : commit 명을 짧게 prefix만 표출

### 2. --graph
```git
$ git log --graph
```
branch commit history를 시각화

### 3. --merges
```git
$ git log --merges
```
merge 커밋만 볼 수 있음  

`--no-merges` : merge 커밋만 skip