---
layout: post
title: "[Git] Commit 메시지 깔끔하게 작성하기"
subtitle: "Commit 메시지 작성"
categories: study
tags: Git
---
## Git Commit Template
 - Commit 메시지에 대한 규칙을 만들어 개발자가 동일한 양식의 Commit 메시지를 작성할 수 있도록 해줌

### 1. 설정하기
#### 1-1. 템플릿 파일 생성
 - Repository에서 사용할 Commit Template 파일 생성
 - 파일 위치는 상관없음
 - 템플릿 파일 작성

	```bash
	<label>:<title>

	# <label> 입력 목록
	#   1. feature : 새로운 기능
	#   2. bug     : 버그 수정
	#   3. update  : 비즈니스 로직 변경
	#   4. refactor: 리팩토링
	#   5. docs    : 문서 (문서 추가, 수정, 삭제)
	#   6. test    : 테스트
	#   7. etc     : 기타 변경사항
	#
	# <title> 규칙
	#   1. 제목 길이 : 50자
	#   2. 제목과 본문 사이 한 줄 띄워 분리
	#   3. 제목 끝에 .(마침표) 금지
	<description>
	# <본문 작성> 규칙
	#   1. 내용의 길이는 한 줄당 60글자 내외에서 줄 바꿈. 한글로 간단 명료하게 작성
	#   2. 어떻게 보다는 "무엇을", "왜" 변경했는지를 작성할 것 (필수)
	<issue-number>
	# <issue> 변경
	#  1. 작성방법 : 키워드 #이슈번호
	#  2. 연관된 이슈 첨부, 여러 개 추가 가능
	#  3. 키워드로 Github 이슈(issue) 닫기 가능
	```  
	> Github는 커밋 메시지에 특정 키워드를 사용해 자동으로 이슈를 종료시키는 기능 탑재  
	> 이 예약어는 커밋 메시지 안의 어느 위치에서나 사용 가능
	> 
	> 키워드 #이슈번호  
	> Github가 이슈 종료로 인식하는 키워드
	> - close
	> - closes
	> - closed
	> - fix
	> - fixes
	> - fixed
	> - resolve
	> - resolves
	> - resolved
	> > 키워드 사용 관례는 아래와 같습니다.  
	> > close 계열 	: 일반 개발이슈  
	> > fix 계열 	: 버그 픽스 또는 핫 픽스 이슈  
	> > resolve	계열: 문의나 요청 사항에 대응한 이슈에 사용  


#### 1-2. config에 템플릿 파일 설정

```bash
git config --global commit.template 템플릿파일경로/템플릿파일.txt
```
- global 옵션을 줄 경우 모든 프로젝트에서 해당 템플릿이 적용됨

### 2. 사용하기
 - `git commit` 메시지를 입력하면 템플릿 파일이 열림  
 - commit 메시지 입력 완료 후 :wq를 통해 저장하면 commit 됨  

![git-commit.png](/assets/img/git-commit-template.png)


Reference:
 - 정아마추어 JEONG_AMATEUR : [지금 당장 좋은 커밋 메시지를 남기는 방법(with Git Commit Template)
](https://jeong-pro.tistory.com/m/207)
 - TOAST : [좋은 git 커밋 메시지를 작성하기 위한 7가지 약속
](https://meetup.toast.com/posts/106)
 - 박준우 블로그 : [좋은 커밋 메시지를 작성하기 위한 커밋 템플릿 만들어보기
](https://junwoo45.github.io/2020-02-06-commit_template/)