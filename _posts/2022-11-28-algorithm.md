---
layout: post
title:  "프로그래머스: 시저 암호(대소문자알파벳배열이용/reduce함수/유니코드메서드/charCodeAt/fromCharCode)"
categories: algo
comments: true
---





프로그래머스: 시저 암호 문제

어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 다른 알파벳으로 바꾸는 암호화 방식을 시저 암호라고 합니다. 예를 들어 "AB"는 1만큼 밀면 "BC"가 되고, 3만큼 밀면 "DE"가 됩니다. "z"는 1만큼 밀면 "a"가 됩니다. 문자열 s와 거리 n을 입력받아 s를 n만큼 민 암호문을 만드는 함수, solution을 완성해 보세요.



---



나는 못풀었다고 한다..... 🫠

어떤개념을 어떻게 써야하는지 1도 모르겠었음 (지금은 알지만...)

근데 블로그 쓰는 사람들중에 해설 친절하게 해놓은 사람 1도 없고 다들 답만 띡- 갖다 붙여놓은거 실화냐고;;;;

---

# CASE 1 (대소문자 합친 배열 사용)

~~~js
const alphabet = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"

function solution(s, n) {
    var answer = '';
    for(let i=0; i<s.length; i++){
        //console.log(s[i])   // AB/z/ a B z 같이 공백이 나오는 경우가 있음
        if(s[i] === ' '){   
            answer += ' '   //공백 삭제
        }else{
            let idx = alphabet.indexOf( s[i] )
            //console.log(s[i], idx)  //A 26 B 27 /z 25/a 0 B 27 z 25
            // const word = idx > 25 ? alphabet.slice( 26, alphabet.length) //어짜피 슬라이스는 2번째 인자가 없으면 끝까지 불러오기때문에 생략가능
            const word = idx > 25 ? alphabet.slice( 26 )    //대문자 묶음
                                  : alphabet.slice (0, 26)  //소문자 묶음
            //console.log(s[i], word) //소문자가 들어있으면 소문자 배열만, 대문자가 들어있으면 대문자 배열만 가져옴
            
            idx = word.indexOf(s[i]) + n;   
            //console.log(word[idx], idx)	//근데 이렇게 던져주면 26(대소문자 관계없이 자신이 묶여있는 인덱스 이상이 되게 되면)이상이게 되면 undefined가 뜨기때문에 26을 빼줘야함. 그래야 다시 a부터 시작함
          
            if( idx >= 26 ){
                idx -= 26
            }
            console.log(s[i], idx, word, word[idx])
            //A 1 ABCDEFGHIJKLMNOPQRSTUVWXYZ B
            //B 2 ABCDEFGHIJKLMNOPQRSTUVWXYZ C
            //z 0 abcdefghijklmnopqrstuvwxyz a
            //a 4 abcdefghijklmnopqrstuvwxyz e
            //B 5 ABCDEFGHIJKLMNOPQRSTUVWXYZ F
            //z 3 abcdefghijklmnopqrstuvwxyz d
            
            answer += word[idx]
        }
    }
    return answer;
}
~~~



대소문자 알파벳을 모아놓고 그걸 배열로 돌려서 푸는 방법

주어진 전달인자중에 공백이 들어가 있는 문자열이 있기때문에 공백처리를 따로 해준다. (`if(s[i] === ' ')`)

`let idx = alphabet.indexOf( s[i] )` : `s[i]`는 몇번째 인덱스를 가지고 있는지 확인

그래서 콘솔에 찍어보면 `1번 : A 26 B 27 / 2번: z 25/ 3번 :a 0 B 27 z 25` 이렇게 나오고 

이 말은 A는 26번째 배열, B는 27번째 배열, z는 25번째, a는 0번째 ... 이런 뜻

그리고 word라는 변수를 만들어서

 `const word = idx > 25 ? alphabet.slice( 26 ) : alphabet.slice (0, 26)` 이렇게 idx가 25보다 크면 대문자로 묶어주고, 아니면 0부터 25번째까지(=소문자까지) 묶어서 사용하겠다고 선언해줌

그리고 `console.log(s[i], word)` 찍어주면 

`'A' 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
'B' 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
'z' 'abcdefghijklmnopqrstuvwxyz'
'a' 'abcdefghijklmnopqrstuvwxyz'
'B' 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
'z' 'abcdefghijklmnopqrstuvwxyz'` 대문자는 대문자 배열을, 소문자는 소문자 배열을 가져옴



그리고 `idx = word.indexOf(s[i]) + n;` 이걸 콘솔에 `console.log(word[idx], idx)` 이렇게 찍으면

`'B' 1
'C' 2
undefined 26
'e' 4
'F' 5
undefined 29`

이렇게 찍음. 문제는 인덱스가 26이상이게 되면 undefined가 뜨기때문에 26을 빼줘야함. 그래야 다시 a부터 시작함

대소문자 관계없이 자신이 묶여있는 인덱스 이상이 되어버리기 때문. 그래서 26이상이면 26을 빼줘야함

`if( idx >= 26 ) idx -= 26 `

그리고 콘솔에 찍으면 `console.log(s[i], idx, word, word[idx])`

`//A 1 ABCDEFGHIJKLMNOPQRSTUVWXYZ B
//B 2 ABCDEFGHIJKLMNOPQRSTUVWXYZ C
//z 0 abcdefghijklmnopqrstuvwxyz a
//a 4 abcdefghijklmnopqrstuvwxyz e
//B 5 ABCDEFGHIJKLMNOPQRSTUVWXYZ F
//z 3 abcdefghijklmnopqrstuvwxyz d`

이렇게  찍음. 그러면 word[idx] 를 answer에 누적해서 더해주면 됨



---

# CASE 2 (대소문자배열 구분)

~~~js
const lower = "abcdefghijklmnopqrstuvwxyz"
const upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

function solution(s, n) {
    var answer = '';
    for(let i=0; i<s.length; i++){
        //console.log(s[i])   // AB/z/ a B z 같이 공백이 나오는 경우가 있음
        if(s[i] === ' '){   
            answer += ' '   //공백 삭제
        }else{
            const word = lower.includes( s[i] ) ? lower : upper
            // console.log(s[i],lower.includes( s[i] ), word)
            //A false ABCDEFGHIJKLMNOPQRSTUVWXYZ
            //B false ABCDEFGHIJKLMNOPQRSTUVWXYZ
            //z true abcdefghijklmnopqrstuvwxyz
            //a true abcdefghijklmnopqrstuvwxyz
            //B false ABCDEFGHIJKLMNOPQRSTUVWXYZ
            //z true abcdefghijklmnopqrstuvwxyz
            let idx = word.indexOf(s[i]) + n;
            if( idx >= 26 ){
                idx -= 26
            }
            console.log(s[i], idx, word, word[idx])
            //A 1 ABCDEFGHIJKLMNOPQRSTUVWXYZ B
            //B 2 ABCDEFGHIJKLMNOPQRSTUVWXYZ C
            //z 0 abcdefghijklmnopqrstuvwxyz a
            //a 4 abcdefghijklmnopqrstuvwxyz e
            //B 5 ABCDEFGHIJKLMNOPQRSTUVWXYZ F
            //z 3 abcdefghijklmnopqrstuvwxyz d
            
            answer += word[idx]
        }
    }
    return answer;
}
~~~



원리는 똑같은데 대문자 소문자 배열을 따로 지정해주고 

`const word = lower.includes( s[i] ) ? lower : upper` 대문자냐 소문자냐를 구별한 다음에 

같은 원리로 아래에 똑같이 써주면 됨.

---

# CASE 3 (reduce활용)

~~~js
const lower = "abcdefghijklmnopqrstuvwxyz"
const upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

function solution(s, n) {
    const answer = s.split('').reduce((acc,cur)=>{
        const word = lower.includes( cur ) ? lower : upper
        let idx = word.indexOf(cur) + n;
        if(idx >= 26) idx -= 26;
        
        return acc + (cur === ' ' ? ' ' : word[idx])
    },"")
    
    return answer;
}
~~~

새로운 개념!! 나도 초기값은 항상 숫자로만 0을 쓰는줄알았는데 문자열도 가능했다는 사실! ⭐️

원리는 다 똑같네.....

어쨌거나 reduce는 값 1개만 리턴하므로 누적값을 구해줘야하는데 acc는 바로 전에 계산했던 수이고

cur는 acc+cur가 계산된 값을 받아서 오는거니까 cur를 가공해서 acc와 더해서 써야함.

근데 cur이 빈칸일수도 있으니까 빈칸이면 빈칸을 더하고 아니면 word[idx]를 해주어야함!

---

# CASE 4(메서드 & 유니코드사용)

~~~js
function solution(s,n){
  let answer = '';
  for(let i=0; i<s.length; i++){
    //A-Z : 65 - 90
    //a-z : 97 - 122
    // console.log(s[i], s[i].charCodeAt() );
    // 'A' 65
    // 'B' 66
    // 'z' 122
    // 'a' 97
    // ' ' 32
    // 'B' 66
    // ' ' 32
    // 'z' 122
    if(s[i] === ' ') answer += ' '
    else{
      let idx = s[i].charCodeAt() + n;
      // console.log(s[i], String.fromCharCode(idx))
      // 'A' 'B'
      // 'B' 'C'
      // 'z' '{'
      // 'a' 'e'
      // 'B' 'F'
      // 'z' '~'
      if( idx > 122 || (idx > 90 && idx - n < 97)){
        // console.log(idx, String.fromCharCode(idx))
        //123 '{'
        //126 '~'
        idx -= 26
      }
      
      answer += String.fromCharCode( idx )
    }
  }
  return answer;
}


solution("AB",1)
solution("z",1)
solution("a B z",4)
~~~

솔직히 이 방법으로 하면 다른거 신경안써도 될거같은데 

여기서 알아두어야할개념 + 알게된 개념

`charCodeAt`
`String.fromCharCode`

이렇게 두개 문자를 유니코드 숫자로 변경해주는 메서드 + 유니코드 숫자를 문자로 변경해주는 메서드이다.

### charCodeAt : 주어진 문자의 유니코드 데이터(숫자)를 반환

~~~
'A'.charCodeAt();	//	65
~~~

### String.fromCharCode : 유니코드 데이터(숫자)를 문자로 반환

~~~
String.fromCharCode(65);	//'A'
~~~



이런 개념이다. 그래서 이 개념을 이용하면 index대신에 사용할 수 있다.

그리고 평소에 알아두면 좋은 개념은 

`A-Z : 65 - 90` 대문자는 65부터 90까지,

`a-z : 97 - 122` 소문자는 97부터 122까지라는거다.

그래서 for문 돌리고 s[i] 를 꺼내면 

~~~js
// console.log(s[i], s[i].charCodeAt() );
// 'A' 65
// 'B' 66
// 'z' 122
// 'a' 97
// ' ' 32
// 'B' 66
// ' ' 32
// 'z' 122
~~~

이렇게 유니코드 번호로 가져오는데, 

~~~js
let idx = s[i].charCodeAt() + n;
// console.log(s[i], String.fromCharCode(idx))
// 'A' 'B'
// 'B' 'C'
// 'z' '{'
// 'a' 'e'
// 'B' 'F'
// 'z' '~'
~~~

이렇게 `s[i].charCodeAt()` 이 유니코드번호에 n을 더해서 idx를 구하고 그걸 콘솔에 찍어보면 위에 처럼 나온다.

보면 `z`가 `{` 혹은 `~` 이게 나오는데, 이것도 번호가 뒤로 밀려서 다른 특수문자들이 나오는거라

~~~js
if( idx > 122 || (idx > 90 && idx - n < 97)){
  // console.log(idx, String.fromCharCode(idx))
  //123 '{'
  //126 '~'
  idx -= 26
}
~~~

만약 유니코드 값이 122를 넘거나 (90이상 && idx-n이 97이하) 라면 idx에서 26을 빼준다.
