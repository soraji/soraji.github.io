---
layout: post
title:  "프로그래머스: 2016 (원하는 날짜 요일구하기) new Date()/getDay/reduce"
categories: algo
comments: true

---



<br>

<br>

프로그래머스 '2016' 문제.

원하는 날짜의 요일을 구하는 문제이다





알아두면 좋은 개념은

~~~js
const now = new Date(2016-04-24)

now	//2016-05-23T15:00:00.000Z
now.getDate();	//24
now.getFullYear();	//2016
now.getMonth() + 1;	//5
now.getDay()	//2	해당요일의 인덱스를 받아옴

typeof new Date() //'object'
typeof Date()	//'string'
~~~



<br>

<br>

나의 해결방안



## case1

~~~Js
function solution(a, b) {
     var answer = '';
     const week = ['SUN','MON','TUE','WED','THU','FRI','SAT']
     answer = week[new Date(`2016-${a}-${b}`).getDay()]

     return answer;
}
~~~

`new Date()`와`getDay` 메서드를 이용했다.

`new Date` 메서드를 날짜로 받고, 

`getDay`는 getDate와는 다르게 요일의 인덱스를 반환해준다. 일요일부터 토요일까지 0 - 6의 숫자로 리턴해준다.

그래서 week라는 배열안에 일-토 까지 선언해준뒤 getDay로 받아온 인덱스값의 숫자를 넣어주면 answer로 리턴해준다

<br>

<br>

<br>

<br>

## case 2 (for문) 

~~~js
function solution(a, b) {
    //기준점이 되는 날짜에서 며칠이 지났는지를 이용해서 구한다
  const week = ['FRI','SAT','SUN','MON','TUE','WED','THU']
  const month = {
    1: 31,
    2: 29,
    3: 31, 
    4: 30,
    5: 31,
    6: 30,
    7: 31,
    8: 31,
    9: 30,
    10: 31,
    11: 30,
    12: 31
  }
  let answer = 0;
  for(let i = 1; i < a; i++){
    answer += month[i]  //5월이 되기 직전의 날짜를 더한다
  }
  answer += (b-1);  //5월이 지난 이후의 날짜를 더한다
  answer = week[answer % 7]
  return answer;
}
~~~

month 객체에 월과 일을 짝지어 선언해준다. 1월은 31일, 2월은 29일… 이런식으로.

그리고 우리가 구하려고 하는건 기준점인 1월 1일 이후부터 구하려고하는 날짜까지 며칠이 지났는지 그 일수를 구해

그 일수를 7로 나눈 나머지가 일주일의 몇번째 인덱스가 되도록 구하면 된다.

처음 for문으로는 5월이 되기 전까지의 1,2,3,4월의 요일을 다 더해서 answer에 더해준다.

`month[i]` 는 month객체의 [i] 번째 키값을 가져온다고 생각하면 됨

그리고나서는 5월1일부터 현재 날짜까지의 일수를 더해 answer에 누적해준다. 

현재날짜의 바로 직전까지 구해야하기 때문에 b-1로 구해준다.

그리고 그 answer를 7로 나눠주고 그 나머지가 week의 인덱스가 된다.



<br>

## CASE 3 (reduce함수)

~~~js
function solution(a,b){
  const week = ['FRI','SAT','SUN','MON','TUE','WED','THU']
	const answer = new Array(a).fill(1).reduce((acc,cur,i)=>{
        //a만큼의 길이값
        const monthNum = cur + i;
        //삼항연산자 사용
        //monthNum이 a월과(1,2,3,4)같지 않은 경우
        return acc + (monthNum !== a ? month[monthNum] : b-1);
    },0)
    
    return week[answer % 7];
}
~~~



<br>

<br>

<br>

## CASE 4



 ~~~js
 function solution(a,b){
   const week = ['SUN','MON','TUE','WED','THU','FRI','SAT']
   
   const days = new Date( 2016 , a-1 , b ).getDay();
 	return week[days];
 }
 ~~~

사실 이게 내가 푼 방법과 가장 비슷하다.

근데 월을 구하는 getMonth() 메서드는 월을 구할때 +1을 해주기 때문에 매개변수로 넣을때는 -1을 해줘야한다.



## CASE 5

~~~js
function solution(a,b){
  const week = ['SUN','MON','TUE','WED','THU','FRI','SAT']
  
  const days = new Date( `2016-${a}-${b}` ).getDay();
  answer = String(answer).slice(0,3).toUpperCase();
	return answer;
}
~~~





 

