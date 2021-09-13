---
layout: post
title:  "JSON 유효성 검사"
categories: js 
comments: true


---

<br>

<Br>

<Br>

<br>

<Br>

~~~javascript
IsJsonString(str) {
    var whatType = JSON.parse(JSON.stringify(str));
    if (typeof whatType == "string") { //유효하지 않은 json
        console.log('this is not json :-( ')
    }else{
        console.log('yay this is json!')
    }
}

async callingAxios(){
    const res =  await axios.get('url')

    try{
        this.IsJsonString(res.data);
    }catch(e){
		console.log('json이 아니에욤 에러처리 해주세요')
    }
}    
~~~



api로 데이터를 받다보면 가끔 json에 벗어나는 형식이 올때가 있다.

그럴때면 데이터 형식에 맞지 않기 때문에 줄줄이 소세지처럼 다 꼬여버리고 아예 화면에 찍을 수 없기때문에

이런 에러가 발생하기 전에 발견하면 좋겠지만... 안되니..(사실 모르겠다. 백엔드에서 json이 아니라는걸 미리 발견하는 방법이 있나? 없을거 같다)

프론트에서 받았을때 json형식이 아니면 에러가 발생해서 slack으로 오게하는 에러처리를 해놓았다.

<br>

우선은 slack으로 알림이 오게하는게 중요한게 아니라 내가 받아온 데이터가 json형식인지, 

유효한지 안유효한지 검사하는게 먼저 아니겠는감? 홍홍

<br>

받아온 data를 res에 넣고 그 res를 받아서 

JSON.stringify()를 해주면 JavaScript 값이나 객체를 JSON 문자열로 변환해준다

참고:https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify

<br>

그리고 나서 변환된 문자열을 또 다시 JSON.parse() 해주면 받은 문자열을 다시 객체로 생성해준다

참고 : https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse

<br>

그 객체의 type이 string이면 json형태가 아닌거다

string이 아니라 객체의 형태로 받아와져야지 json인것임!

<br>

그래서 만약 json이 아니라면 이후에 알아서 에러처리는 원하는대로 하면 된다!

에러처리는 미리미리 해놔야함 (❁ᴗ͈ˬᴗ͈)◞

<br>

<br>

<br>











