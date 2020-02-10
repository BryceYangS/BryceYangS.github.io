yarn 에러 발생 관련
---
`>yarn install`  erorr 발생
error An unexpected error occurred: "https://registry.yarnpkg.com/debug/-/debug-2.6.9.tgz: self signed certificate in certificate chain".

해결방법
`>yarn config set "strict-ssl" false`

vs Code 
---
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


port forwarding
---
iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080

위와같은 명령어를 터미널에서 입력하면 80으로 들어오는 모든 패킷을 8080으로 리다이렉트 처리해버립니다. 80포트로 요청을 받기위해서 웹서버를 8080으로 띄워놓고 위처럼 iptable을 이용해서 80으로 들어오는 패킷을 웹서버가 받도록 하는 것입니다.
