---
layout: post
title:  "Private fork Repository not found"
subtitle:   ""
categories: error
comments: true
---



<br>

<br>

<br>

<br>

<br>

dㅏ 진짜 이거때문에 3일 고생했네

3명이 달라붙었는데도 1도 해결못하다가 결국엔 해결쓰

역시 나 대단해..

(사실은 해결못해서 화난사람 나.. 개발자들은 해결안되면 개빡침..)

켈켈 결국엔 내가 풀어따아

근데 안적어놓으면 백퍼 까먹을거니까 기록해놓기

이렇게 기억이 빠르게 소멸될거같은 기분 오랜만이라 언넝언넝 후딱후딱 적어서 기록해놓도록 하쟈

<br>

<br>

<br>

수업시간에 private repo를 fork해서 가져와야하는게 있었는데

전체수업에서 나만 git clone이 안되는거 ㅜㅜㅜㅜㅜㅜㅜㅜㅜㅜ 20명되는데 나만 안돼 왜 ㅠㅠㅠㅠㅠㅠㅠㅠ

물론 다들 처음하니까 너무 클린해서 잘된건가...

깃 4년넘게 써온사람의 입장에서 뭔가 꼬여도 단디 꼬인거같은데

난 아무것도 한게 없는데..

결국에는 레포 지우고 포크하고 keychain access에서 github계정 다 지우고 컴터 재부팅하고

이걸 도대체 몇번이나 했는지.... 셀수없이 많이했다

근데도 안됨

미친 계속 repo not found만 뜨고..

아니 내 깃에 repo있다고 ㅠㅠㅠㅠ 왜 repo없다고 그러냐고 ㅠㅠㅠㅠㅠㅠㅠㅠㅠㅠ

뭐 근데 보니까 2021년 8월부터 git password없어지고 토큰으로 인증해야한다고 그러긴하던데 결국은 맞는 말이었음

password말고 token으로 함

우선 다른건 모르겠고...

~~~
git remote rm origin
~~~

~~~
git remote add origin  https://깃헙유저네임:깃헙비번@github.com/레포경로.git
git clone https://깃헙유저네임:깃헙비번@github.com/레포경로.git
~~~



이렇게 쳤더니 터미널에

~~~
Support for password authentication was removed on August 13, 2021.
Please see https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
fatal: Authentication failed for '레포.git'
~~~

dㅣ렇게 뜸



보니까 비번 지원이 끊겼다는건데

저거 나오고 

~~~
Username for 'https://github.com':
~~~

이렇게 나오는데 여기서 뒤에다가 유저네임 작성하고 엔터치면

~~~
Password for 'https://soraji@github.com':
~~~

비번치라는데 첨에 깃헙비번치라는줄 알았더니 그게 아니라..;;; 토큰넣으라는 거였음



<br>

<br>

토큰 생성하는법은..

이분거 참고하기

https://hyeo-noo.tistory.com/184

근데 토큰 생성하는것만 따라하고 그 밑에 키체인도 해야하는지 잘모르겠음.. 하긴했는데 안해도 될거같은 느낌

왜냐면 다 따라했었는데 여전히 안됐어가지구..

<br>

<br>

토큰 발행받으면 복사했다가 비번쳐야할떄 컨트롤v만 해주면 됨

엔터치면 복사완료....!

<br>

<br>

하면서 이 글도 참고했음

https://yian.tistory.com/38







