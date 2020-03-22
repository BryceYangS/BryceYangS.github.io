---
layout: post
title: "[etc]Jira issue REST API GET/PUT"
subtitle: "Jira 이슈 조회 및 수정"
categories: study
tags: etc
---


## Jira
- Atlassian이 개발한 이슈 추적 제품
- 버그 추적, 이슈 추적, 프로젝트 관리 기능을 제공하는 소프트웨어
- Atlassian의 여타 플랫폼(Confluence, Bitbucket)과 연동을 할 수 있음
- REST API를 통해 이슈 관리가 가능함

## 1. Issue REST API
### 1.1 Get Issue
`GET` `/rest/api/2/issue/{issueIdOrKey}`

-  issue 정보를 가져올 때 사용


**axios 를 활용한 예시**
```
await axios.get(`https://<지라 URL 입력하세요>/rest/api/2/issue/{issueIdOrKey}`,{
			headers : {
				'Accept': 'application/json',
				'Content-Type': 'application/json'
			 },
			 auth : {
				username: <아이디>, 
				password: <비밀번호>			 
			 }
		}).then(res => {
			console.log(res.data)
		})
```

### 1.2 Edit Issue
`PUT` `/rest/api/2/issue/{issueIdOrKey}`

 - issue 수정할 때 사용
 - **유의사항)**  issue의 필드 형태에 따라 값을 넣어주는 방법이 다름
 
**axios 를 활용한 예시**
```

await axios.put(`https://<지라 URL 입력하세요>/rest/api/2/issue/{issueIdOrKey}`,{
		headers : {
			'Accept': 'application/json',
			'Content-Type': 'application/json'
			},
			auth : {
				username: <아이디>, 
				password: <비밀번호>			 
			 },
			body: `{
			"fields": {
				"<해당 필드>": {"value": "<값>"},
				"<해당 필드>": ["<값>"]
			}
		}`
	}).then(function(response) {
        if (response.status == 200) {
            // SUCCESS
            console.log('jira Issue Update Success')
        }
    })
```

- 이 외에도 다양한 API를 제공해주고 있음. 하단 사이트 참조

## 참조

[Jira REST API Docs](https://docs.atlassian.com/software/jira/docs/api/REST/8.4.2/)
