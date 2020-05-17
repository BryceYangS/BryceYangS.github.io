---
layout: post
title: "[Nodejs]Nodejs 시작하기"
subtitle: "Node.js 시작하기"
categories: study
tags: Nodejs
---

![node-logo.png](/assets/img/node-logo.png)

## **Node.js**

- 기존 WAS가 필요했던 프로젝트와 달리 javascript만으로 서버 구축 가능
- 특징

1. 단일 스레드 모델
   cf) 멀티 스레드 방식 : 프로세스 다수 스레드. 여러 로직 동시 처리 방식 → 복잡한 동기화 문제 발생(동기화 모델, lock 등의 개념에 대한 학습 필요)

2. Non-blocking I/O

- cf) Blocking 방식 : 현재 흔한 프로그래밍 방식. 뒷 줄의 실행이 앞 줄 실행 후 이루어짐. 순차적 진행.
- 함수를 매개변수로 전달 : Non-blocking 방식 동작

```javascript
var fs = require("fs");

fs.readFile("./test.txt", function (err, data) {
  console.log(data);
});
console.log("Hi Node");
```

- - 실행순서 : ./test.txt -> console.log('Hi Node') -> callback 함수 ( console.log(data) ) - 함수 호출 후 리턴값을 받거나, 함수만 호출 시 Blocking 방식 동작 - 시간이 오래 걸리는 작업은 워커 스레드로 진행, 메인 스레드 코드는 계속 진행. 워커 스레드 작업 끝날 시 메인 스레드로 전송

3.  npm
    - Node Packaged Modules
    - Node.js로 만들어진 모듈을 인터넷에서 받아서 설치해주는 패키지 매니저

## **설치하기**

[https://nodejs.org/en/](https://nodejs.org/en/)

- LTS : 안정적인 버전의 nodejs
- Current : 최신 버전의 nodejs
- 노드 설치 완료 후 버전 확인 : cmd 명령프롬프트에서 `node --version` 입력

## **Hello World**

- c:/nodejstest 폴더 생성
- helloworld.js 파일 생성

```javascript
<<helloworld.js>>
const http = require('http');

http.createServer((request, response) => {
	response.writeHead(200, {'Content-Type':'text/plain'});
	response.end('Hello World\n');
}).listen(8090);

console.log('server running 8090 port');
```

- cmd 명령프롬프트 실행 후 서버 시작

```
>cd c:/nodejstest
>node helloworld.js
```

- http://localhost:8090 접속 후 Hello World 출력 확인
- 서버 종료 : cmd 창에서 Ctrl + c 혹은 cmd 종료

[Reference]

- nodejs : [https://nodejs.org/en/](https://nodejs.org/en/)
