---
layout: post
title:  "정수 제곱근 판별"
categories: code
comments: true

---



<br>

<br>

프로그래머스 '정수 제곱근 판별' 문제.

임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.
n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴

<br>

<br>

나의 해결방안

## case1

~~~Js
function solution(n) {
    var answer = 0;
    if(Number.isInteger(Math.sqrt(n)) && Math.sqrt(n) > 0){
        answer = Math.pow(Math.sqrt(n) + 1, 2);
    }else if(!Number.isInteger(Math.sqrt(n))){
        answer = -1;
    }
    return answer;
}
~~~



제곱근/루트를 확인하는 메소드는 `Math.sqrt()` 와 `Math.pow()` 가 있다

---

### Math.sqrt() : 루트값 반환

`Math.sqrt(n)` 는 n의 루트값을 받는다.

---

### Math.pow(제곱할 숫자, 몇번제곱할건지) : 제곱값 반환

`Math.pow(Math.sqrt(n) + 1, 2);` 는 `Math.sqrt(n) + 1` 를 2제곱 한다는 말

---

### Number.isInteger()

`Number.isInteger(확인대상)` 정수인지 확인하는 메서드

<br>

<br>

<br>

<br>

## case 2

~~~js
function solution(n) {
    var answer = 0;
    for(let i = 1; i * i <= n; i++){
        if(i * i === n){
            answer = i +1
          //아래 3가지 방법중에 하나로 return 하기
            return answer * answer 
            return (i+1) * (i+1)
            return (i+1)**2
        }
        
    }
}
~~~





<br>

<br>



<br>

<br>

 





 

