---
layout: post
title:  "공주 구하기 (queue개념)"
categories: algo
comments: true
---

<br>

<br>

정보 왕국의 이웃 나라 외동딸 공주가 숲속의 괴물에게 잡혀갔습니다.

정보 왕국에는 왕자가 N명이 있는데 서로 공주를 구하러 가겠다고 *합니다*. *정보왕국의* 왕은 다음과 같은 방법으로 공주를 구하러 갈 왕자를 결정하기로 했습니다.

왕은 왕자들을 나이 순으로 1번부터 N번까지 차례로 번호를 *매긴다*. *그리고* 1번 왕자부터 *N* 번 왕자까지 순서대로 시계 방향으로 돌아가며 동그랗게 앉게 *한다*. *그리고* 1번 왕자부터 시 계방향으로 돌아가며 1부터 시작하여 번호를 외치게 *한다*. *한* 왕자가 *K*(특정숫자)를 외치면 그 왕자는 공주를 구하러 가는데서 제외되고 원 밖으로 나오게 *된다*. *그리고* 다음 왕자부터 다시 1부터 시작하여 번호를 외친다.

이렇게 해서 마지막까지 남은 왕자가 공주를 구하러 갈 수 있다.

6

5

예를 들어 총 8명의 왕자가 있고, 3을 외친 왕자가 제외된다고 *하자*. *처음에는* 3번 왕자가 3 을 외쳐 *제외된다*. *이어* 6, 1, 5, 2, 8, 4번 왕자가 차례대로 제외되고 마지막까지 남게 된 7 번 왕자에게 공주를 구하러갑니다.

N과 K가 주어질 때 공주를 구하러 갈 왕자의 번호를 출력하는 프로그램을 작성하시오.

▣ 입력설명

첫 줄에 자연수 *N*(5*<=**N**<=*1,000)과 *K*(2*<=**K**<=*9)가 주어진다.

▣ 출력설명

첫 줄에 마지막 남은 왕자의 번호를 출력합니다.

▣ 입력예제 1 : 83

▣ 출력예제 1 : 7

<br>

<br>

<br>

<개념 & 문제 풀이법 생각>

큐라는 자료구조는 먼저들어간게 먼저 나온다. First in first out이라고 함

뭔가 파이프처럼 생겼다고 생각하면 된다.

![스택과큐](/assets/img/2022-12-08/IMG_6957.jpg)

자바스크립트에서 큐는 배열을 이용한다.

<br>

<br>

아 씨 근데 이걸 나 혼자 이 문제를 봤을때 풀 수 있을까.... 나 진짜 절대 못풀거같은데....

설명을 들어도 이해가 안가는데.....? ( ༎ຶŎ༎ຶ )

<br>

<br>

n명의 왕자중 k를 외친 왕자가 제외되고 그 제외된 왕자 다음번부터 다시 1로 시작된다음에 또 k를 외친 왕자가 제외되는 식이다.

큐의 개념을 이용하면 그러니까 앞에서 제외된(=shift) 왕자들을 뒤에다가 push해준다. 왜냐면 3번 왕자가 제외된다는 말은 1,2번 왕자는 살아있다는 소리니까..

그렇게 반복해주면 된다.

<br>

<br>


---

# CASE 1

~~~js
function solution(n, k){
    let answer;
    let queue=Array.from({length:n}, (v, i)=>i+1); //Array.from으로 빈 유사배열객체를 만들어준다. 그리고 길이는 n만큼. 그리고 인자로 v는 배열의 값을 의미하는데 i는 index를 의미하므로 i+1인 1로 시작하는 value로 배열을 채운다
    while(queue.length){	//큐의 길이가 0이 될때까지
        for(let i=1; i<k; i++){	//1번부터 시작해서 제외되는 왕자이전까지(1,2번 왕자)
          queue.push(queue.shift());	//빠지는 왕자들(1,2번 왕자)을 다시 큐로 뒤에다가 푸시해주기
        } 
        queue.shift();	//제외되는 왕자(3번) 제거해주기
        if(queue.length===1){	//큐의 길이가 1이면 
          answer=queue.shift();	//그 왕자가 공주를 구출하러 가야함(shift메서드를 쓰면 shift되는 값을 의미한다)
        } 
    }  
    return answer;
}

console.log(solution(8, 3));
~~~

---

<br>



<br>



<br>

<br>

<br>

<br>

출처 :  [자바스크립트 알고리즘 문제풀이 입문(코딩테스트 대비)](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4/dashboard)를 보고 작성!
