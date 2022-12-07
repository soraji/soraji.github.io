---
layout: post
title:  "프로그래머스: JadenCase 문자열 만들기(substr/toUpperCase/toLowerCase)"
categories: algo
comments: true

---



<br>

<br>

프로그래머스 'JadenCase 문자열 만들기' 문제.

모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 

단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다

<br>

<br>

나의 해결방안



## case1

~~~Js
function solution(s) {
    var answer = [];
    s.split(' ').map((el,i) => {
      answer.push(el.substr(0,1).toUpperCase() + el.substr(1, s.length).toLowerCase())
    })
    return answer.join(' ');
}
~~~

문자열을 split으로(공백기준으로) 나눠준다.

split으로 만들어진 배열을 map을 이용해서 각각의 값과 index를 el과 i로 받아오고

각 값의 1번째 글자(=`el.substr(0,1)`)는 `toUpperCase()`로 대문자로 변경시키고

그 이외의 문자열(=`el.substr(1, s.length)`)은 `toLowerCase()`로 소문자로 변경시킨뒤에 합쳐준다.

그리고 이 더해준 값들을 다시 배열에 넣어준다.

그리고 배열을 합쳐주는 join함수를 써서 값과 공백을 합쳐서(=`answer.join(' ')`) 문자열로 만들어준다.

<br>

<br>

<br>

<br>

## case 2 

~~~js

~~~





<br>

<br>

<br>

<br>

 





 

