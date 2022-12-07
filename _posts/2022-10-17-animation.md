---
layout: post
title:  "[css] animation에 대해 알아보자"
subtitle:   ""
categories: front 
comments: true
---



<br>

<br>

css를 이용해서 애니메이션을 만드는 두 가지 방법

1. transition 속성 활용
2. animation 속성과 keyframe활용

`transition` : 특정한 이벤트를 기점으로 작동(ex. hover)

`animation` : 시작하기 위한 이벤트가 필요없다. 시작, 정지, 반복까지 제어할 수 있다(복잡도 transition보다 높음)

---

`keyframe` : css 애니메이션의 시작, 중간, 끝 등의 중간상태를 정의한다. @를 같이 붙여야 함

~~~css
@keyframe animationName{
	from{
		left: 0;
	}
	to{
		left:200px
	}
}
~~~

<br>

---

### animation 관련 속성들

* `animation-name` : 어떠한 keyframes를 요소에 적용할 것인지 지정한다

  ~~~
  animation-name : moveRight;
  ~~~

* `animation-duration` : 애니메이션을 한번 재생하는데 걸리는 시간 설정

  ~~~
  animation-duration : 2s;
  ~~~

  

* `animation-direction` : 애니메이션 재생 방향을 정의 (정방향/역방향)

  ~~~
  animation-direction : normal (정방향)
  animation-direction : reverse (역방향)
  animation-direction : alternate (정방향. 단, 반복시 정방향/역방향을 번갈아 재생)
  animation-direction : alternate-reverse (역방향. 단, 반복시 역방향/정방향을 번갈아 재생)
  ~~~

  

* `animation-iteration-count` : 애니메이션 재생 횟수를 정의

  ~~~
  animation-iteration-count : infinite
  ~~~

  

* `animation-timing-function` : 애니메이션 재생 패턴을 정의

  ~~~
  animation-timing-funtion : ease-in-out
  ~~~

  

* `animation-delay` : 애니메이션 시작을 얼마나 지연할 지 설정

  ~~~
  animation-delay : 2s
  ~~~

  

<br>

animation 단축 속성

~~~
animation : moveRight 0.4s linear 1x infinite alternate
~~~

`moveRight 0.4s linear 1x infinite alternate` 의 순서대로 적어주어야지만 적용되는게 css에서는 늘 포인트.

순서는 name, duration, timing-function delay iteration-count direction 순이다.

<br>

<br>

---



<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>













