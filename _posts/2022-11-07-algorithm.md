---
layout: post
title:  "배열내 중복값 삭제(map, filter, reduce, set)"
categories: algo
comments: true

---



<br>

<br>

map, filter reduce 메서드 모두 ES6에 새로 추가된 메서드임. 모두 배열에 적용된다는 공통점도 있음.

그리고 모두 순차적으로 모든 요소들에 접근하기 때문에 for문을 따로 작성하지 않더라도

반복문의 원리를 따르기때문에 코드가 엄청 짧아지기도 함



<br>

`map` : 배열 각 요소에 대해 주어진 함수를 수행한 결과를 모아 새로운 배열로 반환함

키는 중복될 수 없기 때문에 이 특성을 이용해 배열 요소를 키(Key)로 해서 맵에 데이터를 넣은 후, 객체 메서드인 keys()를 이용해 키 값만을 배열로 가져온다

~~~js
const arr = ['A', 'B', 'C', 'A', 'B', 'C'];
const onlyObj = {}; // 중복없는 배열 요소만 담는 객체
arr.forEach(el => { 
  onlyObj[el] = true; // {A: true, B: true, C: true }
});
const arrOnly = Object.keys(onlyObj);
console.log(arrOnly);  //[ '1', '2', '3' ]
~~~

source : https://blogpack.tistory.com/1068

---

`filter` : 배열 각 요소에 대해 주어진 함수를 수행한 결과가 true인 요소를 모아 새로운 배열로 반환함

~~~js
const numbers = [1, 1, 2, 2, 3, 4, 5];

const newNumbers = numbers.filter((number, index, target) => {
    return target.indexOf(number) === index;
});

console.log(newNumbers);
// [1, 2, 3, 4, 5]
~~~

---

`reduce` : 배열 각 요소에 대해 reducer함수를 실행하고, ~~배열로 반환~~하는 것이 아니라 **하나의 결과값**으로 반환함

초기값 initialValue가 주어지면, 배열의 0번 index부터 연산을 수행하고, 그 결과를 다음 연산의 인자(accumulator)로 전달.

accumulator에 추가 안된 요소라면 추가하고, 그렇지 않으면 추가하지 않는다.

~~~js
const arr = ['1', '2', '3', '1', '2'];

const initialValue = []
const newArr = arr.reduce((acc, obj) => acc.includes(obj) ? acc : [...acc, obj], initialValue)
console.log(newArr)	//['1','2','3']
~~~

출처 : https://codechacha.com/ko/javascript-remove-duplicates-in-array/

---

`set` : 중복데이터를 허용하지 않음

자바스크립트의 셋(Set) 객체는 맵(Map) 객체에서 값(value)이 없고 키(key)만 있는 것으로 이해하면 된다.

Set 객체 생성자는 배열을 인자로 받아서 중복이 없는 Set객체를 반환한다.

~~~
const arr = ['1', '2', '3', '1', '2'];

const set = new Set(arr);
const newArr = [...set];
console.log(newArr)	//[ '1', '2', '3' ]
~~~

출처 : https://blogpack.tistory.com/1068

<br>

<br>
