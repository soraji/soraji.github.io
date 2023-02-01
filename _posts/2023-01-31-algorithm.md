---
layout: post
title:  "[ 프로그래머스 ] : 3진법 뒤집기(toString, parseInt을 이용한 진수변환) "
categories: algo
comments: true
---

<br>

<br>

https://school.programmers.co.kr/learn/courses/30/lessons/68935

자연수 n이 매개변수로 주어집니다. n을 3진법 상에서 앞뒤로 뒤집은 후, 이를 다시 10진법으로 표현한 수를 return 하도록 solution 함수를 완성해주세요.

<br>

<br>

<br>

<br>

<br>


---

# CASE

~~~js
function solution(n) {
    var answer = 0;
    answer = n.toString(3).split('').reverse().join('');
    return parseInt(answer,3);
}
~~~

그렇게 어려운 문제는 아니었지만, 진법을 변환하는 개념에 대해서 알고 있어야하는 문제였다

매번 쓰던 `toString` `parseInt` 를 이럴때도 쓰는구나, 알았던 날

10진수에서 x진수로 바꾸고 싶으면

~~~javascript
let number = 7;
number.toString(x)
~~~

`toString` 을 이용하면 되고,

x진수에서 10진수로 다시 바꾸려면

~~~javascript
let str = "0021";
parseInt(str,x)
~~~

이렇게 작성하면 된다.



<br>

<br>

<br>

<br>

<br>

<br>



