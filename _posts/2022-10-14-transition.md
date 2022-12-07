---
layout: post
title:  "[css] transition에 대해 알아보자"
subtitle:   ""
categories: front 
comments: true
---



<br>

<br>

* `transition-property` : 어떠한 속성에 transition을 적용할 것인지 지정한다

  ~~~
  transition-property : color, transform
  ~~~

* `transition-duration`: transition에 걸리는 시간을 지정

  ~~~
  transition-duration : 0.2s
  ~~~

* `transition-timing-function`: transition의 속도 패턴을 지정한다

  ~~~
  transition-duration : ease-in-out
  ~~~

  * `linear` : 일정한 속도로 변화
  * `ease` : 시작할 때에는 빨라지다 느려짐
  * `ease-in` : 천천히 시작, 속도를 높여 끝남
  * `ease-out` : 빠르게 시작, 천천히 끝남
  * `ease-in-out` : 천천히 시작, 정상 속도가 됐다가 빠르게 끝남 (가장 많이 쓰임)

* `transition-delay` : transition요청을 받은 후, 실제로 실행되기까지 기다려야 하는 시간의 양을 지정

  ~~~
  transition-delay : 2s
  ~~~

---

트랜지션의 속성을 많기 때문에 한번에 입력하는 법을 알아두면 좋다.

순서가 중요하니 순서도 잘 기억할것

~~~
transition : color(프로퍼티) 0.4s(듀레이션) ease-in-out(타이밍펑션) 1s(딜레이)
~~~



<br>

<br>

<br>

<br>





이렇게 또 하나 배웠군









