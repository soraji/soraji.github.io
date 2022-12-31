---
layout: post
title:  "교육과정 설계 (queue개념)"
categories: algo
comments: true
---

<br>

<br>

현수는 1년 과정의 수업계획을 짜야 합니다.

수업중에는 필수과목이 *있습니다*. *이* 필수과목은 반드시 이수해야 하며, 그 순서도 정해져 있 습니다.

만약 총 과목이 *A*, *B*, *C*, *D*, *E*, *F*, G가 있고, 여기서 필수과목이 CBA로 주어지면 필수과목은 *C*, *B*, A과목이며 이 순서대로 꼭 수업계획을 짜야 합니다.

여기서 순서란 B과목은 C과목을 이수한 후에 들어야 하고, A과목은 C와 B를 이수한 후에 들 어야 한다는 것입니다.

현수가 *C*, *B*, *D*, *A*, *G*, E로 수업계획을 짜면 제대로 된 설계이지만

*C*, *G*, *E*, *A*, *D*, *B* 순서로 짰다면 잘 못 설계된 수업계획이 됩니다.

수업계획은 그 순서대로 앞에 수업이 이수되면 다음 수업을 시작하다는 것으로 *해석합니다*. *수업계획서상의* 각 과목은 무조건 이수된다고 가정합니다.

필수과목순서가 주어지면 현수가 짠 N개의 수업설계가 잘된 것이면 “*YES*", 잘못된 것이면 ”NO“를 출력하는 프로그램을 작성하세요**.**

▣ 입력설명

첫 줄에 한 줄에 필수과목의 순서가 *주어집니다*. *모든* 과목은 영문 *대문자입니다*. *두* 번 째 줄부터 현수가 짠 수업설계가 주어집니다.(수업설계의 길이는 30이하이다)

▣ 출력설명

수업설계가 잘된 것이면 “*YES*", 잘못된 것이면 ”NO“를 출력합니다**.**

▣ 입력예제 1  : *CBA*

*CBDAGE*

▣ 출력예제 1 : *YES*

<br>

<br>

<br>

<개념 & 문제 풀이법 생각>

![스택과큐](/assets/img/2022-12-08/IMG_6957.jpg)

자바스크립트에서 큐는 배열을 이용한다.

<br>

<br>

필수과목과 수강과목을 하나씩 비교해가면서 두개가 일치한다면, 

두개의 배열에서 각각 제거해준다(shift활용)

만약 필수과목과 수강과목이 일치하지 않는다면, 교육과정 순서가 잘못된 것이므로 NO를 리턴한다.

만약 수강과목에는 존재하는데 필수과목이 아니라면 그냥 지나간다.



<br>

<br>


---

# CASE 1

~~~js
function solution(need, plan){
    let answer="YES";	//기본값은 yes로 설정
    let queue=need.split('');	//배열화 시켜주기
    for(let x of plan){	//긴 배열을 겉 for문으로 돌리기
        if(queue.includes(x)){	//만약 필수과목이 들어야하는 과목중에 있다면,(=우선 수강과목에 필수과목이 들어있는지 확인)
            if(x!==queue.shift()){	//큐의 맨 앞의 value가 x와 다르다면 과목의 순서가 다른것이므로
              return "NO";	//잘못된 교육과정이다
            } 
        }
    }
    if(queue.length>0) return "NO";  //만약 필수과목을 수강과정에 넣지 않았다면 큐에는 수강하지 않은 필수과목이 들어있을테니 잘못된 교육과정이다.
    return answer;
}

let a="CBA";
let b="CBDAGE";
console.log(solution(a, b));
~~~

---

<br>

여기서 if문 `if(x!==queue.shift())`의  `queue.shift()` 는 바로 실행되면서 동시에 값을 리턴해주는 메서드인데

콘솔에 찍는다고해도 값만 찍어주는게 아니라 실제로 실행되므로 만약 콘솔에 찍게 되면 

answer에도 영향을 받는다.  

처음에 저렇게 `shift()` 메서드를 if문 자체에 넣어줄 수 있다고 생각도 안해봤는데.... ఠ_ఠ

그래서 아직도 저렇게 if문 안에있으면 헷갈리나...

`if(x!==queue.shift())` 이 말은 `queue.shift()` 로 return된 값이 x와 같지 않다면, 이라는 뜻이다.

<br>



<br>

<br>

<br>

<br>

출처 :  [자바스크립트 알고리즘 문제풀이 입문(코딩테스트 대비)](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4/dashboard)를 보고 작성!
