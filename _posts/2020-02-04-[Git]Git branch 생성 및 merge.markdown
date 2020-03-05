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

**Merge 전으로 reset**
---
```
$ git reflog
8ee26d8 HEAD@{0}: merge opt-express: Merge made by the 'recursive' strategy.
9e75da8 HEAD@{1}: commit: v2
23fa160 HEAD@{2}: checkout: moving from opt-express to master
73aa9b3 HEAD@{3}: commit: f1
23fa160 HEAD@{4}: checkout: moving from master to opt-express
23fa160 HEAD@{5}: checkout: moving from opt-express to master
23fa160 HEAD@{6}: checkout: moving from master to opt-express
23fa160 HEAD@{7}: commit (initial): v1
```
git reflog : 히스토리 확인. 
```
$ git reset --hard HEAD@{2}
```
‘v2’를 commit한 ‘HEAD@{2}’ 시점으로 reset.

- git reset 옵션  

|옵션|설명|
|---|---|
|–soft	|Repository이력만 되돌립니다.|
|[–mixed]|	Stage영역까지만 되돌립니다.|
|–hard|	Working Directory까지 되돌립니다.|

**Reference**
---
-https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge-%EC%9D%98-%EA%B8%B0%EC%B4%88
