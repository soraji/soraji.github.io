---
layout: post
title: 'Error response from daemon: denied: access forbidden'
subtitle: ''
categories: error
comments: true
---

<br>
   
<br>

저번주까지 docker-compose up 잘되다가 같이 일하시는 데브옵스 개발자분이 설정을 뭔가 바꾸신건지

안뜨던 에러가 뜨기 시작했다.

<br>

![dockerlogin](/assets/img/2023-04-10/dockerlogin.png)

<br>

왜 접속권한이 없다는건지… 모르겠지만,

야믈파일에서 뭔가 바뀌어서 그런것 같긴하다.

뭐.. 권한 문제면 권한이 있다는걸 증명만 하면 되니까 뭐...

<br>

찾아보니까 도커registry가 private으로 설정되어있으니 이 권한이 있는 유저만 접근이 가능한데,

그걸 docker login으로 처리해줄 수 있다.

```
docker login registry.gitlab.com/v2/접근하려는레포경로
```

<br>

<br>

저렇게 해주면 이제 `username`이 뭔지 물어보고 `password`를 물어본다

password는 로그인할때 쓰는 비밀번호가 아니라 엑세스 토큰을 넣어야 한다.

나는 user access token발행해놓은게 있어서 그 토큰값을 넣었더니 로그인 처리가 되면서 docker로 띄울 수 있었다.

<br>

<br>

<br>

<br>

<br>

_참조 : https://inkyu-yoon.github.io/docs/Learned/Error/AccessDeniedError_
