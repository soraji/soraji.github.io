---
layout: post
title:  "프로그래머스: 완주하지 못한 선수 (splice/indexOf/sort로비교/filter 사용)"
categories: algo
comments: true

---





<br>

<br>

프로그래머스 '완주하지 못한 선수' 문제.

<br>

<br>

나의 해결방안



## my solution

~~~
나 사실 문제 못풀었다...
filter쓰다가 2중for문쓰다가... 써봤는데 답이 안나와서 고민했는데 시간 얼마 안남아서 포기..ㅜ_ㅜ
~~~

<br>

<br>

------

## CASE 1

```js
function solution(participant, completion) {
    //메서드 없이 푸는 방법
    var answer = {};
    for(let i=0; i< participant.length; i++){
        answer[participant[i]] === undefined
        ? answer[participant[i] ] = 1
        : answer[participant[i] ]++;
    }
    
    for(let i=0; i<completion.length; i++){
        answer[completion[i]]--;
    }
    
    for(let key in answer){
        if(answer[key] === 1){
            return key
        }
    }
        
    
    return answer;
}
```

처음에는 이 문제를 보고 '????뭐지 뭐이렇게... 어렵...뭐지...????' 이런 생각이 들었다(사실은 지금도 마찬가지)

for문을 엄청 쓰네.. 하는 생각 

그리고 객체를 여기에서 쓴다구..? 하는 생각

이렇게 생각해서 하는방법도 있구나.. 라고 신기+재밌었던 해설 ( •⌄• ू )✧

<br>

<br>

우선은 `    for(let i=0; i< participant.length; i++){` 첫번째 for문에서 참가자의 배열만큼 for문을 돌려준다

그리고 answer배열의 인덱스는 몇번째가 될지 모르니 participant의 i번째 배열을 가져온다.

근데 answer배열에는 우선 아무값도 없을뿐더러

participant[i] 의 값들이 'leo', 'kiki', 'eden' 같은 string값들이라 answer의 leo값은.. 당연히 undefined이 뜰 수밖에 없다

그래서 모든 값이 undefined가 뜨게 되는데 그걸 조건문으로 넣는다.

만약 `answer[participant[i]] === undefinded` 이라면 answer라는 Leo 키에 1을 넣어준다는

`answer[participant[i] ] = 1` 코드를 넣어준다.

그러면 콘솔에는 

{ leo : 1 }

{ leo : 1, kiki : 1 }

{ Leo : 1, kiki : 1, eden : 1 }

이런식으로 콘솔에 찍게 되고 숫자가 올라가게 된다.

<br>

<br>

그리고 다음 for문(`for(let i=0; i<completion.length; i++)`)에서는 완주한 사람의 값에 `--` 처리를 해줌으로써 완주한 사람의 value는 0으로 만들고

완주하지 못한 사람의 value는 1로 만들어서 차이를 만들어준다

<br>

<br>

그리고나서 `for in `문으로 객체에서 값을 꺼내준다.

`for(let key in answer)` 으로 `console.log(answer[key], key, answer)` 이렇게 찍어보면

`1 leo {leo : 1, kiki : 0, eden: 0}`

`0 kiki {leo : 1, kiki : 0, eden: 0}`

`0 eden {leo : 1, kiki : 0, eden: 0}`

이런식으로 찍어주게된다. 

그렇다면 `if(answer[key] === 1)` 라는 조건문을 걸어서 완주하지 못한 참가자만 꺼내오면(=return key) 된다

<br>

<br>

## CASE 2 (반복문 줄여서 사용)

```js
function solution(participant, completion) {
    participant.sort((a,b) => a>b ? 1: -1)
    completion.sort((a,b) => a>b ? 1: -1)
    
    // console.log(participant)    //	[ 'eden', 'kiki', 'leo' ]
    // console.log(completion)     //  [ 'eden', 'kiki' ]
    
    for(let i=0; i<participant.length;i++){
        if(participant[i] !== completion[i]){
            console.log(participant[i], completion[i])  //completion는 늘 paricipant의 길이보다 -1이기 때문에 undefined와 비교하면 된다
            answer = participant[i]
            break;  //만약 한번 잘못 틀어지면 계속 어그러지므로 한번 들어가면 break들어가야함(3번 케이스)
        }
    }
    return answer;
}
```

`    participant.sort((a,b) => a>b ? 1: -1)
   completion.sort((a,b) => a>b ? 1: -1)`  이렇게 두 배열을 sort로 정렬해준다는 말은

두 배열을 똑같은 인덱스 값으로 정렬해줬을때 차이가 나는 부분이 있다면 그 부분이 완주자완 참가자를 나누는 기준이라고 생각하면 된다.

`if(participant[i] !== completion[i])` 이 조건문에 걸러진 결과값은

`console.log(participant[i], completion[i])`을 찍어보면 `leo undefined` 로  다를때 완주자가 아닌 사람만 찍어준다.

completion의 길이는 늘 participant보다 -1이다.

그래서 undefined된 completion의 i번째 인덱스와 똑같이 participant의 i번째 인덱스를 이용해

answer에 participant[i] 를 넣어주면된다

<br>

<br>

근데 문제는 3번째 테스트케이스와 같이 completion에 이름이 같은 경우가 있다.

그럼 for문을 돌면서 마지막에 위치한 이름이 들어가게 된다.

그걸 방지하기 위해 break를 넣어준다.

<br>

<br>

<br>

<br>

<br>

<br>

## CASE 3 ( sort )

~~~
function solution(participant, completion) {
    participant.sort((a,b) => a>b ? 1: -1)
    completion.sort((a,b) => a>b ? 1: -1)
    
    const answer = participant.filter((name, i) => {
        return name !== completion[i]
    })
    return answer[0]
}
~~~

아 내가 filter로 이렇게 풀었는데 같지가 않아서 왜 그런가 했더니

sort를 먼저 안해줬구나...

sort를 먼저 해줬으면 같은 인덱스의 값들을 비교할 수 있었을텐데..

다음부터는 값은 같은데 내용이 뒤죽박죽으로 되어있다면 꼭 sort를 먼저 적어줘야지

<br>

---

# CASE exception

~~~js
function solution(participant, completion){
  for(let i=0; i<completion.length; i++){
    if(participant.includes( completion[i]) ){
      participant.splice( participant.indexOf(completion[i]), 1)
    }	
  }
}
~~~

위 코드는 코드실행에서는 통과하지만 효율성테스트에서는 통과하지 못한다

participant의 길이가 너무 길어질 수도 있는 가능성이 있는데(최대10만명) 거기에 메서드를 계속해서 사용해버리니까

메모리 과부하가 일어나서 그렇다.

그치만 이 문제는 그런게 아니더라도 splice나 indexOf의 개념을 알기에 괜찮아서 기록해둔다

<br>

<br>

<br>

---

### splice

`splice` : 배열에 사용 가능한 메서드

1. 지정한 배열의 특정 구간 요소를 제거할 수 있다.
2. 지정한 배열의 특정 구간에 요소를 추가할 수 있다.

~~~Js
const arr = [1,2,3,4,5]

//1번요소 : 기준으로 삼고자하는 인덱스요소, 2번요소: 기준으로 삼은 요소에서 몇개의 요소를 제거해 줄 것인지
//mutable : 원본데이터를 변환시킨다 (=sort도 mutable)

//제거방법(2개의 매개변수)
//1번 인덱스부터 1개의 요소를 제거해준다.
arr.splice(1, 1)	//[2]

//추가방법(3개의 매개변수)
//1번 인덱스부터 0개의 요소를 삭제하고 'a'(=3번째 매개변수)를 추가한다
arr.splice(1, 0, 'a')	//[1, 'a', 2, 3, 4, 5]

//1번 인덱스를 기준으로 2개의 요소를 삭제하고 'a'를 추가한다
arr.splice(1, 2, 'a')	//[1, 'a', 4, 5]
~~~



Splice 메서드는 정말 많이 쓰이는 메서드인데, 이제 명확하게 좀 알겠다

배열에서 추가도하고 삭제도 할 수 있는데 삭제를 하려면 2개의 매개변수만 적어주면 되고,

추가를 하려면 3개의 매개변수를 넣어주면 된다.

`기준이 되는 인덱스`, `몇개를 삭제할 것인지`, `추가할 내용(3번째 인자는 추가하는 경우가 아니면 생략가능)` 

이렇게만 알고 있으면 splice는 자주 쓸 수 있을것 같다 

