# 앞으로 써야할 내용



## Git : 나의 Fork & Fork의 original repository와 동기화

[\[Git\] Fork 한 repository 최신으로 동기화하기](https://json.postype.com/post/210431)


## yarn 에러 발생 관련

`>yarn install` erorr 발생


error An unexpected error occurred: "https://registry.yarnpkg.com/debug/-/debug-2.6.9.tgz: self signed certificate in certificate chain".

해결방법

`>yarn config set "strict-ssl" false`

## vs Code 
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
    

2. 조회 및 삭제 방법
https://srzero.tistory.com/entry/Ubuntu-Iptable-nat-%EC%A1%B0%ED%9A%8C-%EB%B0%8F-%EC%84%A4%EC%A0%95-%EC%82%AD%EC%A0%9C

## ec2 & nginx & mariaDB설치
https://daddyprogrammer.org/post/2348/aws-ec2-install-nginx-mariadb/

## Linux
$ sudo su -  : root 계정으로 전환
$ 

**port forwarding**
1. 등록
iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080


위와같은 명령어를 터미널에서 입력하면 80으로 들어오는 모든 패킷을 8080으로 리다이렉트 처리해버립니다. 80포트로 요청을 받기위해서 웹서버를 8080으로 띄워놓고 위처럼 iptable을 이용해서 80으로 들어오는 패킷을 웹서버가 받도록 하는 것입니다.

## AWS ec2 / nginx : http -> https redirect
https://perfectacle.github.io/2017/10/05/https-with-elb/
