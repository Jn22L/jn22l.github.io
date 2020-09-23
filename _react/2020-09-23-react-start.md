---
title: "React 시작 - Create React App 셋팅"
date: 2020-09-23 09:23:00 -0400
categories: react
---

1. node.js , vscode 설치

2. npm install -g create-react-app

 * 인증서 오류 발생시 ( npm ERR! code SELF_SIGNED_CERT_IN_CHAIN) 다음을 먼저 실행
 npm config set registry="http://registry.npmjs.org/"
 npm config set strict-ssl false -g 

3. create-react-app my-react-app

4. cd my-react-app

5. npm start