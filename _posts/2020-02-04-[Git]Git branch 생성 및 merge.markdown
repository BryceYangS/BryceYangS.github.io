---
layout: post
title: "[Git] branch 생성 및 branch 병합하기(merge)"
subtitle: "Git branch 생성 및 merge"
categories: study
tags: Git
---
**branch 생성**
---
- `git branch <branchname>`
	- 생성하고자 하는 branch 명을 입력
- `git branch`
	- 브랜치 목록 확인
	- 앞에 * 붙어있는 것이 현재 HEAD가 가리키는 branch

```
$ git branch
  master
* opt-express
```
- `git add .` 와 `git commit -m "<commit message>"`는 기존과 동일
- `git push origin <branch명>`
	- 해당 branch에 push를 해줌.

**branch 병합**
---
- `git checkout master`
	- master 브랜치로 HEAD가 가리키도록함.
- `git merge <commit>`
	- 지정한 <commit> 내용이 HEAD가 가리키고 있는 브랜치에 병합.
	
```
$ git checkout master
$ git merge opt-express -m 'merge'
```
