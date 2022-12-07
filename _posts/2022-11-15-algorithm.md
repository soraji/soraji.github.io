---
layout: post
title:  "프로그래머스: 콜라츠추측"
categories: algo
comments: true

---



<br>

<br>

프로그래머스 '정수 콜라츠추측' 문제.

~~~
1-1. 입력된 수가 짝수라면 2로 나눕니다. 
1-2. 입력된 수가 홀수라면 3을 곱하고 1을 더합니다. 
2. 결과로 나온 수에 같은 작업을 1이 될 때까지 반복합니다. 
~~~

주어진 수가 6이라면 6 → 3 → 10 → 5 → 16 → 8 → 4 → 2 → 1 이 되어 총 8번 만에 1이 됩니다. 위 작업을 몇 번이나 반복해야 하는지 반환하는 함수, solution을 완성해 주세요. 단, 주어진 수가 1인 경우에는 0을, 작업을 500번 반복할 때까지 1이 되지 않는다면 –1을 반환해 주세요.

<br>

<br>

나의 해결방안

우선은 for문에 몇번이라는 횟수가 없었기 때문에 난 while을 사용했음

근데 보니까 500이라고 조건을 줘도 됐으니 for문을 쓸걸.. 하는 생각이 들었다

## case1

~~~Js
function solution(num) {
    var answer = 0;
    while(answer <= 500){	//500번 미만이면
      if(num === 1) return answer;	//계속나누나보면 num이 1이 되었을때
          if(num % 2 === 0){
            num = num / 2
          }else{
            num = num * 3 + 1;
          }
          answer += 1;
        }
     return -1	//500번 이상이면 -1리턴
}
~~~



생각해보면 해설을 딱히 뭐 많이 할만한 문제는 아님

주석처리한것만 봐도 이해감

<br>

<br>

<br>

<br>

## case 2 (for문)

~~~js
for(let i=0; i<500; i++){
  //반복이 종료되는 조건 추가
  if(num === 1){
    return answer;    //만약 num이 1이면 함수 스탑  
  } 
  answer++;

  if(num % 2 === 0){    //짝수인경우
    num = num / 2
  }else{
    num = ( num * 3 ) + 1
  }
}
    
return -1;
~~~





<br>

<br>

## CASE 3 (메서드) forEach문

~~~js
const result = new Array(500).fill(1).forEach((el)=>{
  //for는 return이나 break를 사용해서 깨주면 되는데 
  //forEach는 재귀함수로 본인이 다시 호출되기때문에 조건문을 걸어준다
  if(num !== 1){  
    answer++;
    num = num % 2 === 0 ? num / 2 : (num * 3) + 1 ;    
  }
})

return num === 1 ? answer : -1
~~~

사실은 forEach가 효율적으로 보이지만 사실은 그렇지 않다.

메서드를 사용하면 코드가 짧아져서 좋아보일 수 있지만 매번 그런것만은 아니기에 메서드 사용을 집착하는것도 좋지 않다고 한다

<br>

<br>

 





 

