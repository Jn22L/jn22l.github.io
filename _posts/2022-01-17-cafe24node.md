---
title: "카페24 Node 호스팅 Git 커밋하기"
date: 2022-01-17 17:06:00 -0600
categories: cafe24 호스팅 설정
tags: git
---
카페24 Node 호스팅 Git 커밋하기
---
Cafe24 Node 호스팅 Git 커밋하기 

1. git ssh 생성 ( CLI or Git GUI 선택 )
```
 CLI 에서 만들기
  ssh-keygen -t rsa -C "이메일@주소"

 Git GUI 에서 만들기
  메뉴 : Help > Show SSH Key : Generate Key 클릭

 생성파일
    폴더위치 : C:/Users/계정/.ssh
    id_rsa.pub : 공개키 -> 카페24 Public Key 등록
    id_rsa     : 개인키 -> 암호설정
```    

2. 생성한 Public Key(id_rsa.pub)를 카페24 등록후 앱에 할당
```
관련메뉴
  호스팅관리 > Public key 관리 : Public Key 등록
  호스팅관리 > 앱 생성/관리  : [Key 할당] 버튼 -> 등록한 Public Key 를 앱에 할당해줌
```

3. 소스 수정후 순서대로 git 명령 실행
```
*로컬 저장소 생성

mkdir myrepo
cd myrepo

*사용자 설정

git config --global user.name "이름"
git config --global user.email "이메일@주소" 

*명령어로만 진행시 알수 없는 오류발생하여 다음과 같이 하는게 편함

1. init, remote add 는 Git GUI 에서 작업
    Create New Repository 
    Remote > Add
2. add, commit 은 vscode 에서 작업
    commit 할때 node_modules 폴더까지 해야함 : 카페24에서 빌드 안해줌.
3. push 는 터미널 에서 작업
    git push --set-upstream 원격저장소 master

*원래 명령어 정리 - 참고만 하기 ( push 에서 에러발생, 원인은 remote add 가 문제인듯 ?? 모르겠음 )

git init
git add web.js 
git commit -m "first commit"
git remote add origin 원격저장소
git push --set-upstream 원격저장소 master
```

4. 모두 업로드 되었으면, cafe24 앱 중지후 실행

5. web.js 예제 ( 카페24 노드호스팅 기본실행 파일명은 web.js )
```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello World !");
});

app.listen(process.env.PORT || 8080, () => {
  console.log(`Example app listening at http://localhost:${process.env.PORT || 8080}`);
});
```

끝!
