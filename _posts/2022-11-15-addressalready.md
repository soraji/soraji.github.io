---
layout: post
title:  "Mac에러 address already in use :::3000"
categories: error
comments: true


---

<br>

<Br>

진짜 컴터에 도커, 몽고디비, 웹, node 서버 다 띄우니까 에러가..안날래야..안날수가 없는거 아니냐구...

<br>

한번도 못봤던 문제들이 떴다..

vscode에서 node 로 js파일을 실행하는데

~~~
$ node index.js
~~~

아래 에러가 떴다..

<Br>

~~~
Error: listen EADDRINUSE: address already in use :::3000

Emitted 'error' event on Server instance at:
    at emitErrorNT (node:net:1399:8)
    at processTicksAndRejections (node:internal/process/task_queues:83:21) {
  code: 'EADDRINUSE',
  errno: -48,
  syscall: 'listen',
  address: '::',
  port: 3000
}
~~~

3000포트를 내가 쓰고 있다고...?

다른 서버 띄워놓은게 없는게 뭐지..?

vscode 껐다켰다 얼마나 많이 했는지 모르겠다.. 결국에는 에러를 검색해봤는데

어쩄거나 서버가 겹쳐있는거라고 그랬다.

그래서 살아있는 서버를 찾으려고했더니

~~~
$ lsof -i TCP:3000
~~~

이렇게 하면 3000번 포트를 쓰고있는 살아있는 모든 서버가 나온다

~~~
$ kill -9 PID(번호로되어있음)
~~~

이렇게 kill로 강제종료해준다.

그리고나서 node index.js하니까 서버연결 잘 됨

<br>

<Br>

출처 : https://jootc.com/p/201912253249

<br>

<Br>



<br>

<Br>



<br>

<Br>

