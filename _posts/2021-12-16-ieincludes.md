---
layout: post
title:  "IE에서는 includes()함수가 작동하지 않아요(.includes not working in IE)"
categories: js 
comments: true


---

<br>

<Br>

<Br>

<br>

<Br>

오 역시 기대를 저버리지 않는 IE...!!!!!!!!!!!!!!!

크롬에서는 되는데 ie에선 안되는 문제들 역시 벗어나질않는다^^...



IE를 제외한 모든 브라우저에서 includes함수를 인지하는데 IE는 인지못함^^

그래서 `indexOf` 함수 써주어야 한다

includes함수는 문자열에 포함되어있는지 아닌지 바로 boolean으로 결과를 return하는데 비해

indexOf는 존재하면 정수, 존재하지 않으면 음수로 결과를 return한다.

그래서 아래의 실질적으로 비교할때는 아래 코드처럼 비교하면 된다.

결과값은 똑같음

~~~javascript
function existCheck(){
    const str = "result remains";
    if(str.includes('result')){
      console.log('yay! result is in the sentence!')  
    }else{
      console.log('result is NOT in the sentence :-( )')  
    }
    //IE에서는 아래의 indexOf를 써야합니당
    if(str.indexOf('result') != -1){	//존재하지 않지 않으면 => 존재하면...(한국어는 어렵다)
      console.log('yay~~~result가 문장에 있습니다')  
    }else{
      console.log('result가 문장에 없어요 ㅠㅠ')   
    }
} 
~~~



위의 `if(str.indexOf('result') != -1)`  말고도 `if(str.indexOf('result') >= 0)` 이런식으로 작성해도 된다

무궁무진함..... 포인트는,

존재한다 =>0이상의 숫자로 return 

존재하지 않는다 => -1로 return



MDN includes() 자료

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes#polyfill

MDN indexOf() 자료

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf

<br>

<br>

<br>











