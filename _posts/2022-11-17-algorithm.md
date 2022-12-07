---
layout: post
title:  "프로그래머스: 내적"
categories: algo
comments: true

---



<br>

<br>

프로그래머스 '없는 숫자 더하기' 문제.

a와 b의 [내적](https://en.wikipedia.org/wiki/Dot_product)을 return 

~~~
a[0]*b[0] + a[1]*b[1] + ... + a[n-1]*b[n-1]
~~~



<br>

<br>

나의 해결방안



## case1

~~~Js
function solution(a, b) {
    var answer = 0;
    for(let i=0; i<a.length; i++){
      answer += a[i]*b[i];
    }
    return answer;
}
~~~



<br>

<br>

<br>

<br>

## case 2 

~~~js
function solution(numbers) {
    const answer = new Array(9).fill(1).reduce((arr, cur, i)=>{
        const num = cur + i;
        return acc + (numbers.includes(num) ? 0 : num)//누적값이 담김
    },0);   //초기값이 0으로 적어주는 것이 answer = 0과 같은말
    
    
    return answer;
}
~~~



아 리듀스 진짜 봐도봐도 모르겠다...

엄청 강력하다고 하는데 대강 1개의 값만 도출된다는 정도만 알겠다..ㅜ.ㅜ 

오늘 푼 문제들이 다 reduce를 이용해서 풀수 있는 문제들이었는데..

(사실 배열이면 다 reduce이용할수있다)

원리가 이해가 안감 ㅠㅠ 다른 사람들 해설해놓은거 읽어야지...

암만 풀어도 내 해결방안으로 풀듯....ㅠㅠ reduce어떻게 생각해내냐구 ㅠㅠ...

<br>

<br>

<br>

<br>

 





 

