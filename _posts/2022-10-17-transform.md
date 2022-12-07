---
layout: post
title:  "[css] transform에 대해 알아보자"
subtitle:   ""
categories: front 
comments: true
---



<br>

<br>

`transform` : 요소에 이동, 회전, 확대축소, 비틀기 등의 변형 효과를 줄 수 있다

* `translate` : 요소의 좌표를 움직일 수 있다. x축으로 x만큼,y축으로 y만큼 이동.

  ~~~css
  transform : translate(20px, 25%)
  ~~~

* `translateX` / `translateY`: 요소의 x/y축 좌표를 n만큼 움직일 수 있다

  ~~~css
  transform : translateX(20px)
  transform : translateY(20px)
  ~~~

* `scale(x,y)` : x축으로 x만큼,y축으로 y만큼 요소를 축소 혹은 확대

  ~~~css
  transform : scale(0.75, 1.1)
  ~~~

* `skew(x,y)` : x축으로 x만큼,y축으로 y만큼 요소를 기울인다 (deg는 degree(각도)이다)

  ~~~css
  transform : skew(15deg, 10deg);
  ~~~
  
* `rotate(n)` : 요소를 n만큼 회전시킨다

  ~~~css
  transform : rotate(45deg);
  ~~~
  
  

<br>

<br>

quiz ) 요소를 75도 회전시키고, y축 방향으로 120ox 이동시키려면 ? 

~~~
transform : rotate(75deg) translateY(120px)
~~~

quiz ) 요소를 x축 방향으로 30도, y축 방향으로 10도 기울이고 45도 회전시키려면?

~~~
transform : skew(30deg, 10deg) rotate(45deg)
~~~

quiz ) 요소를 y축 방향으로 0.75 축소시키고 x축방향으로 20도 기울이려면?

~~~
transform : scaleY(0.75) skewX(20deg);
~~~



<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>













