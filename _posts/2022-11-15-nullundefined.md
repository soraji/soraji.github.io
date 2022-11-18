---
layout: post
title:  "null과 undefined 차이"
categories: js 
comments: true


---

<br>

<Br>

# null과 undefined 차이





## `undefined`

* 변수를 선언하고 값을 할당하지 않은 상태(자료형이 없는 상태)
* typeof undefined를 콘솔에 찍으면 undefined가 출력된다
* 선언한 후에 값을 할당하지 않은 변수나 값이 주어지지 않은 인수에 자동으로 할당
* 값이 지정되지 않은 경우를 의미



## `null` 

* 변수를 선언하고 빈 값을 할당한 상태(빈 객체)이다. 
* typeof null를 콘솔에 찍으면 object가 출력된다 (=> 개발자들의 실수였음 ㅎㅎㅎ)
* 어떤 값이 **의도적으로** 비어있음
* 해당 변수가 어떤 객체도 가리키고 있지 않다는 것을 의미

<br>

<br>

~~~
typeof null // 'object'
typeof undefined // 'undefined'
null === undefined // false
null == undefined // true
null === null // true
null == null // true
!null // true
isNaN(1 + null) // false
isNaN(1 + undefined) // true
~~~



<br>

<br>

<br>

출처 : https://2ssue.github.io/common_questions_for_Web_Developer/docs/Javascript/13_undefined&null.html#null

<br>

<br>

<br>







