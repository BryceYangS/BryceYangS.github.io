# 앞으로 써야할 내용

## Git

#### 나의 Fork & Fork의 original repository와 동기화

[\[Git\] Fork 한 repository 최신으로 동기화하기](https://json.postype.com/post/210431)

#### git 원격지 브랜치 삭제

작성자 : 정광섭 - 2014, 6월 27
git 에서 remote branch delete 하는 방법.

삭제할 브랜치 이름은 feature/TEST-860 이다

방법 1
git push origin --delete feature/TEST-860

방법 2
git branch -d feature/TEST-860
git push origin feature/TEST-860

방법 3
git checkout develop
git branch --delete feature-01

(git branch -D feature-01 ---> 강제 삭제)
git push origin :feature-01 --> 원격 브랜치 삭제

Ref
How to delete a Git branch both locally and remotely?

#### git remote에서 branch 삭제했는데 local 에서 조회가 될 경우

git remote update 후
git branch -a 를 통해 branch 를 조회할 때 remote 에서 삭제된 branch들이 조회됨

해결 방법

```
git fetch --all --prune
또는
git remote prune origin
```

###git 에서 리모트 리포지토리를 로컬로 덮어쓰기
실서버 소스를 약간 수정한 뒤 git pull을 했더니, conflict가 일어나서 패닉상태가 되었을때.. 이 두 커멘트면 해결이 가능하다.

$ git fetch origin
$ git reset --hard origin/master

참고로 인터넷상에, git pull 커멘드를 이용해 강제로 pull을 받는 방법이 소개되어 있는데, pull은 git fetch와 git merge origin/master를 동시에 해주는 커멘드로써, 로컬에서 소스와 merge할 일이 없으면 되도록이면 쓰지 않는게 좋다.

## yarn 에러 발생 관련

`>yarn install` erorr 발생

error An unexpected error occurred: "https://registry.yarnpkg.com/debug/-/debug-2.6.9.tgz: self signed certificate in certificate chain".

해결방법

`>yarn config set "strict-ssl" false`

## vs Code

```
{
    "editor.fontFamily": "'D2Coding ligature', Consolas, 'Courier New', monospace",
    "editor.fontSize": 15,
    "editor.fontLigatures": true,
    "terminal.integrated.shell.windows": "C:\\Windows\\System32\\cmd.exe",
    "window.zoomLevel": 1,
    "editor.minimap.enabled": false,
    "workbench.colorTheme": "One Dark Pro",
    "workbench.iconTheme": "material-icon-theme",
    // js
    "javascript.suggestionActions.enabled": false,
    "javascript.format.enable": false,
    "javascript.validate.enable": false,
    // format
    "editor.formatOnSave": true,
    "[jsonc]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.defaultFormatter": "dbaeumer.vscode-eslint"
    },
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true,
    },
    // eslint
    "eslint.enable": true,
    "eslint.format.enable": true,
    "eslint.alwaysShowStatus": true,
}
```

## ec2 & nginx & mariaDB설치

https://daddyprogrammer.org/post/2348/aws-ec2-install-nginx-mariadb/

## Linux

$ sudo su -  : root 계정으로 전환
$

**port forwarding**

1. 등록
   iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080

위와같은 명령어를 터미널에서 입력하면 80으로 들어오는 모든 패킷을 8080으로 리다이렉트 처리해버립니다. 80포트로 요청을 받기위해서 웹서버를 8080으로 띄워놓고 위처럼 iptable을 이용해서 80으로 들어오는 패킷을 웹서버가 받도록 하는 것입니다.

2. 조회 및 삭제 방법
   https://srzero.tistory.com/entry/Ubuntu-Iptable-nat-%EC%A1%B0%ED%9A%8C-%EB%B0%8F-%EC%84%A4%EC%A0%95-%EC%82%AD%EC%A0%9C

## AWS ec2 / nginx : http -> https redirect

https://perfectacle.github.io/2017/10/05/https-with-elb/

## How to install node and mongodb on Amazon EC2

https://github.com/SIB-Colombia/dataportal-explorer/wiki/How-to-install-node-and-mongodb-on-Amazon-EC2

## Front End 공부

https://poiemaweb.com/

## Mongo DB

https://www.tutorialspoint.com/mongodb/mongodb_environment.htm

#### 몽고DB 윈도우 설치

https://javacpro.tistory.com/64

#### mongo DB 명령어

https://velopert.com/545
https://sjh836.tistory.com/100

#### mongo DB 데이터 Dump

mongodump --out <경로> --db <db명>

#### mongo DB dump file을 가지고 DB 추가

mongorestore --db <db명> <경로> --drop

#### AWS EC2 Mongo DB 반영

mongorestore -u <어드민아이디> -p <어드민비번> --authenticationDatabase admin 다른 방법은 동일

## javascript

### javascript 공부

http://www.jstips.co/en/javascript/

### javascript 관련

```javascript
// 길이 9 짜리 배열을 만들고 각 배열의 값을 "1"로 초기화함
const a = Array(9).fill("1");
//결과 : (9) ["1", "1", "1", "1", "1", "1", "1", "1", "1"]
```
### Javascript 비동기 / Promise

https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/
https://joshua1988.github.io/web-development/javascript/promise-for-beginners/#promise%EA%B0%80-%EB%AD%94%EA%B0%80%EC%9A%94


### javascript push/concat 차이
 - push : 기존 배열을 복사한 후 원소 추가 수행.
 - concat : 기존 배열 복제하지 않음.

### [javascript] Shallow Copy  vs Deep Copy 
 - Shallow Copy : Call by reference
    - javascript 는 객체의 주소값을 할당
 - Deep Copy : Call by Value
    - deep copy 하는 법 : JSON.parse(JSON.stringify(target-object))


## 웹개발 공부

https://www.codecademy.com/

## AWS

### AWS에 FileZilla로 SFTP에 접속하기

https://nickjoit.tistory.com/150

## Vue.js & Express로 백/프론트 분리하기

https://medium.com/hivelab-dev/vue-express-mysql-part1-98f68408d444



## GeoCoding

https://scyoon.tistory.com/103

## Python

### package 설치 시 SSL 에러

https://surpreem.com/%ED%8C%81-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%8C%A8%ED%82%A4%EC%A7%80-%EC%84%A4%EC%B9%98%ED%95%A0-%EB%95%8C-ssl-%EC%9D%B8%EC%A6%9D-%EC%98%A4%EB%A5%98-%ED%95%B4%EC%A0%9C-%EB%B0%A9%EB%B2%95/

http://blog.naver.com/PostView.nhn?blogId=kyung4502&logNo=221498420654
