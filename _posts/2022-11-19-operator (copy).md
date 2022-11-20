---
layout: post
title:  "==와 ===의 차이점"
categories: js 
comments: true


---

<br>

<Br>

# ==와 ===의 차이점



==는 양 옆의 값을 비교하기 전에 강제 형 변환(Type Coercion)를 수행한다 

강제 형변환 과정을 통해 피 연산자들을 공통 타입으로 만들고 

그 안에 있는 값만을 비교하는, '느슨한 비교'를 한다.

하지만 === 의 경우, 강제 형변환 과정을 수행하지 않는 '엄격한 비교'이기 때문에 false 가 뜬다. 

엄격한 비교이기에 이름도 'strict equality'임

~~~
let a = 1; 
let b = "1"; 
console.log(a == b); // true 
console.log(a === b); // false

console.log(null == undefined); // true 
console.log(null === undefined); // false 

console.log(NaN == NaN); // false 
console.log(NaN === NaN); // false 
~~~



<br>

<br>

출처 : https://steemit.com/kr-dev/@cheonmr/js-operator

출처 : https://all-dev-kang.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C

<br>

<br>

<br>







