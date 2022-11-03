---
layout: post
title:  "[JS] Async & Await (어씽크어웨이트) & URI"
categories: js 
comments: true

---



<br>

<br>

---

## 동기 / 비동기

통신을 해서 데이터를 받아오기 위해서는 요청(request)과 응답(response)의 과정이 있어야한다.

근데 이 요청과 응답의 시간차이(물론 그마저도 어마무시하게 빠르겠지만..)떄문에 에러가 나기도 한다.

그래서 그 순서를 잘 생각해서 구조를 짜주어야 하는데, 그떄 발생할 수 있는 문제들을 해결할떄 사용하는게 async await임

어씽크 어웨이트를 사용하기 위해서는 동기/비동기의 개념, 왜 동기를 쓰고 왜 비동기를 쓰는지 알아야함.

<br>

<br>

동기(synchronous ) : 서버컴퓨터가 작업이 끝날때까지 기다리는 통신

비동기 (asynchronous): 서버 컴퓨터가 작업이 끝날때까지 기다리지 않는 통신(서버에 요청저장이 될때까지 기다리지않고 다른 작업 진행)

그렇담 비동기는 언제쓸까?

요청들이 서로 기다릴 필요가 없을때 (= 동시에 여러 일을 할때) 쓴다.

<br>

기본적으로 요즘 라이브러리는 비동기로 작동된다. (axios lib)

그래서 응답도 Promise로 가져오는데..

~~~js
const data = axios.get('https://koreanjson.com/posts/1')
console.log(data)	//Promise
~~~

<br>

<br>

만약 기본적으로 비동기로 작동하는 통신을 동기적으로 이용하고 싶으면 await를 사용하면 되는데,

async 라는 함수로 감싸야지만 await를 사용할 수 있음

~~~js
async function synch(){
	const data = await axios.get('https://koreanjson.com/posts/1')
	console.log(data)	//{id:1, title:"제목" ...}
}
~~~



<br>

<br>

근데 자바스크립트에서는 호이스팅의 문제로 함수선언문보다 함수표현식으로 써준다

~~~js
//이거말고
function onClickAsync() {
    const result = axios.get('https://koreanjson.com/posts/1');
    console.log(result);
};
  
 //이렇게써줌 
const onClickAsync = () => {
    const result = axios.get('https://koreanjson.com/posts/1');
    console.log(result);
};
~~~

<br>

<br>

<br>

<br>

~~~js
//이거말고
async function onClickSync() { 
    const result = await axios.get('https://koreanjson.com/posts/1');
    console.log(result);
    console.log(result.data.title);
};


//이렇게써줌
const onClickSync = async () => {	//=>함수중복선언 문제 피하기 위함
    const result = await axios.get('https://koreanjson.com/posts/1');
    console.log(result);
    console.log(result.data.title);
};
~~~



<br>

<br>

---

## URI & URN & URL

URL

~~~
https://google.com?search=철수
~~~

URN

~~~
google.com?search=철수#aaa
~~~

URI

~~~
https://google.com?search=철수#aaa
~~~



