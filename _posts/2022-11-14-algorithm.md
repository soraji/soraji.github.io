---
layout: post
title:  "정수 제곱근 판별"
categories: algo
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
    if(Number.isInteger(Math.sqrt(n)) && Math.sqrt(n) > 0){	//n의 루트값이 정수이고, n의 루트값이 0보다 크다면
      	//answer는 n의 루트값에 1을 더한값의 2제곱
        answer = Math.pow(Math.sqrt(n) + 1, 2);
    }else if(!Number.isInteger(Math.sqrt(n))){	//n의 루트값이 정수가 아니라면 ex)1.87678
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
    for(let i = 1; i * i <= n; i++){	//i는 1부터 시작, i*i(=i의 제곱)가 n보다 작거나 같을때까지 i를 +해준다
        if(i * i === n){	//i의 제곱이 n과 같다면
            answer = i +1	//answer는 i(=n의 루트값) +1
          
          	//아래 3가지 방법중에 하나로 return 하기
            return answer * answer //(i+1)의 제곱
            return (i+1) * (i+1)	//(i+1)의 제곱
            return (i+1)**2	//(i+1)의 제곱
        }
        
    }
}
~~~





<br>

<br>



<br>

<br>

 





 

