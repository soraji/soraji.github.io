---
layout: post
title:  "[ 프로그래머스 ] : 이진 변환 반복하기(toString, parseInt을 이용한 진수변환) "
categories: algo
comments: true
---

<br>

<br>

https://school.programmers.co.kr/learn/courses/30/lessons/70129

0과 1로 이루어진 어떤 문자열 x에 대한 이진 변환을 다음과 같이 정의합니다.

1. x의 모든 0을 제거합니다.
2. x의 길이를 c라고 하면, x를 "c를 2진법으로 표현한 문자열"로 바꿉니다.

예를 들어, `x = "0111010"`이라면, x에 이진 변환을 가하면 `x = "0111010" -> "1111" -> "100"` 이 됩니다.

0과 1로 이루어진 문자열 s가 매개변수로 주어집니다. s가 "1"이 될 때까지 계속해서 s에 이진 변환을 가했을 때, 이진 변환의 횟수와 변환 과정에서 제거된 모든 0의 개수를 각각 배열에 담아 return 하도록 solution 함수를 완성해주세요.

<br>

<br>

<br>

<br>

<br>


---

# CASE

~~~js
function solution(s) {
    let answer = [0,0];
  
    while(s !== '1'){
      s = s.split('');
      let temp = s.filter(x => x === '1').length;
      answer[0] ++;
      answer[1] += s.length - temp;
      s = temp.toString(2);
    }
    return answer;
}
~~~



진수를 구할때는 `toString(x)` 을 사용하면 되는데(x = 구하려는 진수)

평소에는 진짜 `while` 쓸일이 없는데 알고리즘만 하면 while을 진짜 많이 쓰게 된다.

매번 잘 안써서... 아직도 잘 좀 못쓰겠다. 손에 안익음

1. s가 1이 아닐때만 while문이 실행되도록 함. 
2. 모든 문자열을 기준으로 split을 해주고
3. filter로 0이 아닌 문자열(= '1'인 경우만)만 뽑아서 배열로 만들고, 그 길이를 구한다
4. answer의 0번째 인덱스는 while문이 실행되는 횟수만큼 올리면 되고
5. answer의 1번째 인덱스는 제거해야하는 0의 갯수이므로 처음 매개변수로 받았던 s의 길이에서 1로만 구성된 문자열의 길이를 누적해서 빼주면 된다.
6. 그리고 s는 answer와는 별개로 2진수로 변환해준다.
7. s가 1이 아니면 계속해서 while문을 돌거임



<br>

<br>

<br>

<br>

<br>

<br>



