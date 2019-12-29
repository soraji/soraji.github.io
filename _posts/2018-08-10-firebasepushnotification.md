---
layout: post
title:  "firebase를 이용한 푸시알림 [1편] - 파이어베이스 소개 및 설치"
subtitle:   "firebase"
categories: Developer
tags: firebase
comments: true
---

[제가 편집한 깃허브](https://github.com/soraji/firebase-noti.git) 에 들어가면 코드있습니당 =)

# 파이어베이스 예제

[Ire의 블로그](https://github.com/ireade/simply-notify)에서 레포지토리 클론또는 다운을 받는다. 다운받은 코드들은 이미 완성작이라서 코드들이 다 적혀있고, 이미 프로젝트도 다 파베프로젝트로 설정되어있다. firebase.json을 보면 알수있으니 말이다. 

그리고... 정말이지 너무나도 완벽한 Ire 의 영상!!!! [두번보고 세번보고 열번 보기!!](https://www.youtube.com/watch?v=XdzXaW8IbBM)



### 파이어베이스 프로젝트

---

그래서 마크업과 css,img만 다시 복사해서 가져오는것으로 하고 나머지는 처음부터 다시 하는걸로 시작해보려고 한다. 

맨 처음부터 한다면 파베에서 새 프로젝트를 추가해주어야한다. 새 프로젝트를 추가하는 방법들은 많이 나와있으므로 여기서는 생략!

만약 firebase가 설치가 안되어있다면

~~~
npm i -g firebase-tools
~~~

로 설치해주면 된다.

설치가 됐다면, 혹은 되어있다면

~~~
firebase init
~~~

명령으로 만드려고 하는 프로젝트를 파이어베이스 프로젝트로 설정해준다.

1. *Are you ready to proceed?* =>Yes
2. *Which Firebase CLI features do you want to set up------?* => Database, Function, Hosting 을 선택한다. 선택하는것은 spacebar를 누르면 선택 및 해지를 할 수 있다. 엔터는 다음단계로 넘어가는것!
3. *Select a default Firebase project for this directory:* => 내가 파이어베이스에 만들어놓았던 프로젝트 목록이 뜨는데 본인이 연결하고 싶은 프로젝트를 선택하여 엔터!
4. *what file should be used for Database rules?* => 디폴트값으로 엔터
5. *what language would you like to use---?*=>Javascript
6. *Do you want to use ESLint---?*=>No
7. *Do you want to install dependencies with npm now?* =>Yes (선택하면 설치가 시작된다)
8. *what do you want to use as your public directory?*=> .(루트)
9. Configure as a single-page app?=> No
10. FIREBASE initialization complete!! 파베 초기화성공=)!





### 파이어베이스 프로젝트 구조

---

제대로 설치가 되었다면 파베의 프로젝트 구조를 먼저 살펴보고 가자.

기본적으로 **index.html**과 **404.html**이 생성되어있고, 

**.firebaserc**라는 FIREBASERC파일이 있는데, 열어보면 파베콘솔에서 본인이 연결시켰던 프로젝트명이 적혀있다. 

**database.rules.json** 파일은 database의 rules 내용이 들어있다. 기본적으로 아마 

~~~json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
~~~

내용이 적혀있을텐데 이 말은 auth되지 않은사람(인증되지 않은사람)은 읽지도, 쓰지도 못한다. 라는 내용이다. 콘솔에서도 컨트롤할수있다. 후에 보안에 관해서 수정을 필요로 하는 내용이다.

**firebase.json** 파일은,

~~~json
{
  "database": {
    "rules": "database.rules.json"
  },
  "hosting": {
    "public": "public",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ]
  }
}
~~~

database, hosting에 관한 내용들을 보여주는 파일이다.

**function 폴더안에 index.js** 파일은 나중에 notification에 관한 js들을 적어야할 js이다. 





### 예제 시작하기

---

폴더루트에 index.html과 404.html이 생성되어있을텐데, 마크업은 가져오기로 하였으니, Ire가 작성한 index.html은 가져오기로 한다. 그런데 이 마크업도 완성작이니 만약 코드를 다 쳐보고 싶다, 하는 사람은 아래 코드를 복사하면 될것 같다. 

~~~html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Simply Notify</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="css/reset.css">
  <link rel="stylesheet" href="css/style.css">
</head>
<body>
  <div class="wrapper">
    <main>
      <header>
        <h1>Simply Notify</h1>
        <p><em>A simple application to send notifications to everyone subscribed. It's simple. It's useless. It's simply notify.</em></p>
      </header>
    </main>

    <footer>
      <small><a href="https://github.com/ireade/simply-notify">View Source</a></small>
    </footer>
  </div>

  <!-- Firebase SDK -->
  <script src="/__/firebase/4.1.3/firebase.js"></script>
  <script src="/__/firebase/init.js"></script>

  <script src="js/main.js"></script>
</body>
</html>
~~~

코드를 열어보면, index.html에 

```html
<script src="/__/firebase/4.1.3/firebase.js"></script>
<script src="/__/firebase/init.js"></script>
```

이라고 이상한경로가 적혀있는데, 이것들은 firbase SDK이고, npm 으로 설치를 한다면 저절로 생성되는것이다. 자동으로 sdk가 설치되는것이니 따로 설치를 안해줘도 된다.

서버가 있으면 서버에서 돌리고 만약 없다면 

~~~
firebase serve
~~~

를 입력해서 local을 띄운다.

위 코드를 넣어보면 

![simplenoti1](/assets/img/simplenoti1.PNG)

라고 아무 버튼이 뜨지 않은걸 볼 수 있다.







---

다음편은 파베 auth를 이용하여 인증하는 법에 대한 글입니다 =)









