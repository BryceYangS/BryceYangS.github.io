---
layout: post
title: "[Git]Git Merge: Squash & Rebase"
subtitle: "Git의 Merge 이해하기"
categories: study
tags: Git
---
> Git의 Merge 이해하기 : Squash & Rebase


## 1. 들어가기 전에. ***Fast-Forward란?***
- FF, 즉 빨리감기라고 이해하면 된다.
- 아래 `master`와 `hotfix`는 fast-forward 관계이다. `master`를 `hotfix`에 머지할 경우 `fast-forward Merge`가 발생하며 커밋 히스토리가 발생하지 않고 브랜치 포인터가 최신 커밋으로 이동한다.  

![fast-forward](/assets/img/git/fast-forward.png)

**1.fast-forward 관계 O**
![fast-forward](/assets/img/git/fast-forward-ok.png)

커밋B의 히스토리에 커밋A의 히스토리가 모두 포함되어 있다.


**2.fast-forward 관계 X**
![fast-forward](/assets/img/git/fast-forward-no.png)

커밋B의 히스토리에 커밋A의 히스토리가 포함되어 있지 않다.（A의 커밋이 포함되어 있지 않다.）


## 2.Merge
`git merge`는 `fast-forward` 또는 `3-way merge`를 행한다.
- fast-forward Merge는 브랜치 포인터를 최신 커밋으로 이동만 하면 되는 경우 발생
- 3-way Merge는 merge commit 을 발생시켜 브랜치를 병합하는 방식

Git의 Merge는 `merge` 명령어와 `rebase` 명령어를 통해 진행하며, 두 명령어의 작동방식이 다르기 때문에 전략에 따라 선택적으로 사용한다.


### 2-1. git merge의 --no-ff 옵션
![no-ff](/assets/img/git/merge-noff.png)

3-way merge 방식으로 fast-forward 관계와 상관없이 `머지 커밋`을 생성한다.  
`git merge` 명령어는 기본 옵션이 `--ff` 로 fast-forward 머지를 하기 때문에 fast-forward인 경우 머지 커밋을 생성하지 않는다.  
`--no-ff` 옵션을 사용해 머지 커밋을 **강제 생성**한다.

```git
$ git checkout master
$ git merge --no-ff noff
```
![noff](/assets/img/git/noff.png)



## 3. Squash Merge
> git merge \-\-squash <branch명>

![squash](/assets/img/git/squash.png)

- a,b,c 커밋이 합쳐져 새로운 커밋을 만들어 merge한다.
- 새로운 커밋의 *parent는 1개*로 merge 대상(merge from - to 에서 from 에 해당되는 브랜치)의 기록은 추적 불가하다

```git
$ git checkout main
$ git merge --squash feature-branch
$ git commit -m "squash merge message"
```

## 4. Rebase Merge
> git rebase <branch명>

![rebase](/assets/img/git/rebase.png)

브랜치의 commit들이 합쳐지지 않고 main 브랜치에 추가된다.  
추가된 commit history는 하나의 parent를 가진다.

- 주의사항
    + Rebase는 기존 커밋을 그대로 사용하는 것이 아니라 내용은 같지만 다른 커밋을 새로 만듦
    + 따라서,기존 push 한 것을 `git rebase`로 바꿔서 push할 경우 기존 push 한 것에 대해 pull했던 동료들은 또 다시 merge를 진행해야 하는 상황이 발생할 수 있다.

```git
$ git checkout rebase-branch
$ git rebase main
$ git checkout main
$ git merge rebase-branch
```


[References]
- TOAST 블로그 : [GitHub의 Merge, Squash and Merge, Rebase and Merge 정확히 이해하기](https://meetup.toast.com/posts/122)
- [https://im-developer.tistory.com/182](https://im-developer.tistory.com/182)
- [Difference Between Git Rebase and Merge
](http://www.differencebetween.net/technology/difference-between-git-rebase-and-merge/)
- Git : [3.2 Git 브랜치 - 브랜치와 Merge 의 기초
](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge-%EC%9D%98-%EA%B8%B0%EC%B4%88)
-[https://stackoverflow.com/questions/34019195/git-pull-doesn-t-fast-forward-merge-even-though-there-are-no-conflicts](https://stackoverflow.com/questions/34019195/git-pull-doesn-t-fast-forward-merge-even-though-there-are-no-conflicts)