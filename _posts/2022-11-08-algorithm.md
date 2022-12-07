---
layout: post
title:  "프로그래머스 가운데 글자 가져오기 slice"
categories: algo
comments: true

---



<br>

<br>

프로그래머스 '가운데 글자 가져오기'

문제 ) 단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요. 단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.

나눈 값을 인덱스로 이용해서 문자열의 i번째 인덱스를 가져오면 된다.

아래는 내가 푼 방법....ㅠ.ㅠ

~~~js
function solution(s) {
    var answer = '';
    const arr = s.split('');
    for(let i=0;i<arr.length;i++){
        if(arr.length % 2 === 0){
            answer = arr[Math.floor(arr.length/2)-1];
            answer += arr[Math.floor(arr.length/2)];
        }else{
            answer = arr[Math.floor(arr.length/2)];
        }
    }
    
    return answer;
}
~~~

ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ진짜 내 코드는 왜이렇게 지저분한걸까..

해설보면 세상 깔끔하게 되어있어서 너무 부럽다..

부끄럽지만.. 그래도 내가 어떻게 생각하고 풀었는지 기억할 수 있게 코드 첨부...ㅜ.ㅜ

<br>

<br>

~~~js
return s.length % 2 !== 0
        ? s[Math.floor(s.length / 2)]
        : s.slice(s.length / 2 - 1, s.length / 2 + 1);
~~~



<br>

<br>

클린한 코드를 보고 내 코드와 비교해보면

굳이 배열로 만들어주지 않아도 되는데 배열로 만들어주어서 코드 길이가 길어졌고,

삼항연산자를 사용하면 더 깰꼼하게 작성할 수 있을것 같다.

그리고 지금 slice사용하는 문제 3번째인데 아직도 slice를 쓰면되겠다, 라는 생각이 전혀 들지 않는다.......

배열에서 어떤 특정 범위의 값을 빼와야한다면 다음번엔 반드시 slice나 splice를 이용해서 가져와봐야겠다

다짐! (‘•̀ ▽ •́ )φ
