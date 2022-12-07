---
layout: post
title:  "문자열 다루기 기본"
categories: algo
comments: true

---



<br>

<br>

프로그래머스 '문자열 다루기 기본' 문제.

문자열 s의 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수

<br>

<br>

내 답안...은 모든 케이스 테스트를 통과하지는 못했다 ㅜ.ㅜ...

도대체 뭐가 문제야아아아아아~~~

숫자인지 문자인지 확인하는 방법은 isNaN를 사용하면 된대서 사용했는데 그래두 안됨 ㅠ.ㅠ 뭔가 꼬였나부당

~~~js
function solution(s) {
    var answer = true;
    
     for(x of s){
         if((s.length === 4 || s.length === 6) && !isNaN(s) && Number(s) > 0 ){
             return true
         }else{
             return false
         }
     }  
}
~~~

<br>

<br>



근데 역시나 isNaN으로 Not a Number을 써서 확인하는게 맞았다.

우선은 답은 

~~~js
function solution(s) {
    var answer = true;
    if(s.length !== 4 && s.length !== 6) return false;
    
     for(let i=0; i<s.length; i++){
         if(isNaN(s[i])) return false; //문자열이 하나라도 들어있는 파라미터는 바로 Return 시킴으로서 함수 종료;
         //isNaN('123') //false나오는 이유는 isNaN검사를 할때 123을 처음부터 Number로 변환시킨값을 isNaN체크를 하기 때문
     }
     return answer;
}
~~~

이렇게 인데,

`    if(s.length !== 4 && s.length !== 6) return false;` 이건 이제 4글자나 6글자가 아니면 그냥 false로 반해버린다.

만약 글자가 4글자나 6글자라면

문자를 split을 사용해서 배열로 만들어준다.

그리고 for문을 사용해서 s문자열의 길이만큼 돌린다.

그리고 문자열의 한 글자씩(=`s[i]`) `isNaN` 에 넣어서 이 글자가 숫자인지 문자인지 확인한다.

그래서 문자가 하나라도 들어있는 단어는 바로 false를 return해서 함수를 종료시킨다.

이때 `isNaN('123')` 이 false가 나오는데,

이때 false가 나오는 이유는 isNaN검사를 할때 123을 처음부터 Number로 변환시킨값을 isNaN체크를 하기 때문

<br>

<br>

<br>

<br>

---

### filter 사용

~~~js
function solution(s) {
    var answer = true;
    answer = s.split('').filter(num => {
        return isNaN(num);	//[ 'a' ]
    })
    return answer.length === 0;
}
~~~

우선 문자열을 split으로 배열로 만들어주고 filter를 사용해서 값을 빼온다.

그리고 isNaN으로 각 값이 숫자인지 아닌지 확인한다.

그리고 필터롤 통해서 `return isNaN(num)` 을 하게 되면 데이터가 숫자가 아닌 문자 타입만 남긴다. 

NaN값인 데이터만 남긴다. 결국에는 `[ 'a' ]` 이것만 남음.

이 answer에 NaN이 존재하면(=문자가 있으면) answer.length가 0이 아니므로 false가 되고

answer에 숫자만 존재하면 answer.length가 0이므로 true가 된다.



<br>

<br>



<br>

<br>

 





 

