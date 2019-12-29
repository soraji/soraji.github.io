---
layout: post
title:  "firebase를 이용한 푸시알림 [5_2편] - 메세지 보내기"
subtitle:   "firebase"
categories: Developer
tags: firebase
comments: true
---



## 파이어베이스 5_2는 메세지 보내기를 성공해서, 그 글을 남긴 글입니다..! 

## 보니까 버전이 굉장히 중요하더라구요, 제가 예제를 참조하던 [Ire유투브](https://www.youtube.com/watch?v=rumJsnHbXsI&t=659s)에서 본 코드로 하면 당연히 될거라고 생각했는데, 계속 functions 로그에 error가 떠서 왜 똑같이 했는데 에러가 뜨는걸까 엄청나게 고민하다가 결국에는 알아낸게 파베에서 업데이트를 했더라구요!!!! 베타버전에서 버전 1.0으로요. 그래서 코드도 달라지고, 데이터 보내는 형식도 달라지고.. 그래서 결국에는 코드가 달라진것 같아요. 뭐.. 지지고 볶고 파베에서 제공하는 코드와 원래코드를 섞어서 짜기는 했는데, 치명적인 오류가 한번만 오고 두번째부터는 안온단 말이죠..? 구글을 아무리뒤져봐도, 스택오버플로우를 아무리 뒤져봐도 이런내용이 없어서.. 이건 파베팀에게 물어봐야할것 같아요ㅠㅠㅠ진짜 거의 다왔는데... 심지어 성공도 했는데.. 이거가지고 꼬박 4일을 매달렸어요! 진심으로 가치가 있었기를 바라면서.. 파베팀에서 답장이 오면 새로 글로 써보려구요..! 우선은 제가 한데까지 코드 남겨야겠어요.



[Cloud 함수용 Firebase SDK 이전 가이드: 베타에서 버전 1.0으로 이전](https://firebase.google.com/docs/functions/beta-v1-diff) 이글은.. 그냥 정말 달달외울정도로 계속 읽으세요.... functions사용하실거라면...!



## 1단계(functions/index.js수정)

------

파이어베이스 function을 이용해서 할건데 폴더루트에 가보면 자동으로 function폴더가 생성이 되어있을것이다. 왜냐면 우리가 맨처음에 파베프로젝트를 만들었을때 function도 선택해 주었기 때문에!

function폴더에 들어가보면 package.json, node-modules, index.js가 있는데 index.js에다가 우리가 메세지를 보낼때 필요한 모든 function들을 적어줄 것이다.

index.js를 열어서

~~~javascript
'use strict';

const functions = require('firebase-functions');
var admin = require("firebase-admin");
var serviceAccount = require("./push-web-app-52e8d-firebase-adminsdk-3wttu-38b026cfea.json");
admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: "https://push-web-app-52e8d.firebaseio.com"
});

~~~



을 넣어준다. admin은 관리자계정으로 임포트하도록 도와주는것이고 

~~~javascript
admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: "https://push-web-app-52e8d.firebaseio.com"
});
~~~

은 초기화를 시켜주는 코드이다. 



## 2단계(function exports)

---

~~~javascript
exports.sendNotifications = functions.database.ref('/notifications/{notificationId}')
    .onWrite((change, context) => {

      const followerUid = context.params.notificationId;

      // If un-follow we exit the function.
      if (!change.after.val()) {
        console.log(followerUid);
      }
~~~

그리고 함수를 사용할 수 있도록 export를 해주는데, sendNotifications라는 함수를 export해준다. 

  

여기서 중요한게 전에 코드와 달라진점중에 제일 중요한게 있다면 데이터를 가져오는 형식이 달라졌다는거다. 예전에는

~~~
event.data()
~~~

였는데, 이제는 

~~~
data
~~~

혹은 

~~~
context
~~~

로 바뀐것이다. 그래서 안먹었던 거였어....

  

  

SDK 버전 1.0.0에는 `firebase-admin` 5.11.0이 필요한데, 굉장히 중요한거라고 한다.......

~~~
npm install firebase-functions@latest --save
npm install firebase-admin@5.11.0 --save
~~~

npm 설치해주시고, firebase cli로 마찬가지로

~~~
npm install -g firebase-tools
~~~

다시 업데이트하고...





그래서 초기화할때도 예전에는

~~~
//베타버전
admin.initializeApp(functions.config().firebase);
//1.0버전
admin.initializeApp();
~~~



이렇게 바뀌었다.. 

## 아무리생각해도 파베 문서 업데이트의 날짜를 잘 확인하는게 중요한것 같다...!!!!!ㅠㅠㅠㅠㅠㅠㅠ 



## 3단계

---

그리고 그 밑에다가

~~~javascript
const getDeviceTokensPromise = admin.database().ref(`/currentToken`).once('value');
      return getDeviceTokensPromise.then((data)=>{
        if(!data.val()){
          console.log("data don't exist")
        } 

        const snapshot = data.val();
        // console.log(snapshot);
        const tokens = [];

        for (let key in snapshot){
          tokens.push(snapshot[key].currentToken); 
        }

        // console.log("this is TOKEN: " + tokens);
        const SNAPSHOT = change.after.val();
        const msg = {
            notification: {
              title: SNAPSHOT.user,
              body: SNAPSHOT.message,
              icon: SNAPSHOT.userProfileImg
            }
          };
        console.log(msg);

        admin.messaging().sendToDevice(tokens, msg)
        .then(function(response){
          console.log("success!",response);
        })
        .catch(function(error){
          console.log("fail to send :( ",error);
        })
      });
~~~

코드를 적어준다.

~~~javascript
const getDeviceTokensPromise = admin.database().ref(`/currentToken`).once('value');
~~~

코드는 파베디비에 있는 데이터구조를 보면 

![simplenoti9](/assets/img/simplenoti9.PNG)

currentToken값이 프로젝트 밑에 있기때문에 그 값을 가져온다.

데이터를 실질적으로 가져오는 코드는 

~~~
const snapshot = data.val();
        // console.log(snapshot);
        const tokens = [];

        for (let key in snapshot){
          tokens.push(snapshot[key].currentToken); 
        }

        // console.log("this is TOKEN: " + tokens);
        const SNAPSHOT = change.after.val();
        const msg = {
            notification: {
              title: SNAPSHOT.user,
              body: SNAPSHOT.message,
              icon: SNAPSHOT.userProfileImg
            }
          };
        console.log(msg);


~~~

인데, console.log(snapshot)을 찍어보면

![snapshot](/assets/img/snapshot.png)

으로 data.val(), 그러니까 currentToken DB밑에값에 있는 value를 볼 수 있다. 계속해서 바뀌는 값들(-LK0wh4uL8zbA7_ZSrye) 이값들을 보고 snapshot이라고 하는데, 이 snapshot밑에 currentToken, uid가 들어있는것을 확인할 수 있다. 

  

그러면 위 코드를 뜯어보자.

tokens라는 비어있는 변수를 하나 선언해준다. 

~~~
const tokens = [];
~~~

그리고 for문을 돌린다

~~~
for (let key in snapshot){
          tokens.push(snapshot[key].currentToken); 
        }
~~~

왜냐면 이글을 구독하는 사람이 1사람이면 상관없겠지만 100명이면, 그와 일치하는 토큰값을 찾아야하기 때문이다. key를 선언하고 key번째의 snapshot의 currentToken값을 새로만든 배열 tokens에 밀어넣는다(push). 

그리고 

~~~
console.log("this is TOKEN: " + tokens);
~~~

값을 쳐보면

![thisistoken](/assets/img/thisistoken.png)

이라고 토큰값만 가지고 올 수 있다.



## 4단계(notification 내용 및 보내기)

---

~~~
const SNAPSHOT = change.after.val();
        const msg = {
            notification: {
              title: SNAPSHOT.user,
              body: SNAPSHOT.message,
              icon: SNAPSHOT.userProfileImg
            }
          };
        console.log(msg);
~~~

실제로 msg내용을 보내는 코드는 위의 코드인데, SNAPSHOT을 change.after.val(); 로 선언해주었다. 1.0버전에서 변경된 데이터값을 change.after형식으로 부른다고 하니까 말이다.



이제는 실제로 메세지를 보내보자.

~~~
admin.messaging().sendToDevice(tokens, msg)
        .then(function(response){
          console.log("success!",response);
        })
        .catch(function(error){
          console.log("fail to send :( ",error);
        })
~~~

바로 위에다가 tokens값, msg값을 잘 받아오는지 확인하기 위해 위에다가 console로 tokens, msg찍어봤는데 잘 들어온다. 만약 들어오지 않는다면 디버깅으로 찾아야한다. 나같은 경우도 처음에는 분명 콘솔에 success가 뜨는데 아무내용도 알림으로 오지 않았던 이유가 여기있었다. msg내용이 null값은 아닌데 정확히 선언되지 않아서 [object]로 계속 떠서.. 여튼 바로 위코드에서 콘솔에 잘 들어온다면 메세지가 오는데 이상은 없을것이다..

![notisuccess](/assets/img/notisuccess.png)

그리고 콘솔에 느리게 뜬다. 내용을 보내고 나서 한 7-8초 있다가 로그에 뜬다. 

그리고 나서 조금 있다가

![sorasuccess](/assets/img/sorasuccess.png)

이렇게 알림이 온다.

  

  

  

  

  

  

  

  

  

  

 

이 알림을 오게하기 위해 도대체 몇번의 디플로이와 디버깅을 했는가.... 고맙게도 파베콘솔에 찍어주더라

![deploydebug](/assets/img/deploydebug.png)

400번 가까이 디플로이+디버깅해봄

이정도면 집착이라 할만 하다. 그래도 했으니까..... 한거에 의미를 두고!

길게 천천히 쓰려했는데 지쳐서 못쓰겠다 ㅠㅠㅠㅠ 이렇게 알림오는거까지 해보았다!!!