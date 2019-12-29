---
layout: post
title:  "firebase를 이용한 푸시알림 [5_1편] - 메세지 보내기(이 글은 디버깅한 과정을 지우고싶지 않아서 그냥 놔둔글입니다)"
subtitle:   "firebase"
categories: Developer
tags: firebase
comments: true
---

## 파이어베이스 5편 수정했습니다. 제가 여태 실습하던 코드는 베타버전이었어요. 그래서 코드수정이 있었던걸로 보여집니다. firebase공식문서에도 친절하게 잘 알려주고는 있지만 그래도 아직 오류가 있어서 파베팀에게 물어봐야할것 같아요! [Cloud 함수용 Firebase SDK 이전 가이드: 베타에서 버전 1.0으로 이전](https://firebase.google.com/docs/functions/beta-v1-diff) 이 제목으로 글을 써놨는데 뭐 당최 알수가 있어야죠... 여튼 푸시메세지 보내는것은 성공했는데, 2번째부터 메세지가 또 오지 않는다는.. 치명적인 버그? 오류? 가 또 발생해요..! 굉장히 길게 적어놓았던 편이라, 이걸 다 삭제하기에는 제가 디버깅을 몇번했는지 남기고 싶어서 원글은 그대로 놔두고 새로 작성하려구요! 다시 5편을 작성했습니다..!





[제가 편집한 깃허브](https://github.com/soraji/firebase-noti.git) 에 들어가면 코드있습니당 =)

드디어!!! 메세지를 보내는걸 해볼거다!

## 1단계(functions/index.js수정)

---

파이어베이스 function을 이용해서 할건데 폴더루트에 가보면 자동으로 function폴더가 생성이 되어있을것이다. 왜냐면 우리가 맨처음에 파베프로젝트를 만들었을때 function도 선택해 주었기 때문에!

function폴더에 들어가보면 package.json, node-modules, index.js가 있는데 index.js에다가 우리가 메세지를 보낼때 필요한 모든 function들을 적어줄 것이다.

index.js를 열어보면

~~~javascript
const functions = require('firebase-functions');

// // Create and Deploy Your First Cloud Functions
// // https://firebase.google.com/docs/functions/write-firebase-functions
//
// exports.helloWorld = functions.https.onRequest((request, response) => {
//  response.send("Hello from Firebase!");
// });
~~~

라고 적혀있을텐데 여기에다가 firebase admin을 추가해주어야한다.

~~~javascript
const admin = require('firebase-admin');
~~~

를 추가한다. 추가하는 이유는 package.json에가보면 이미 dependencies에 firebase-admin이 있는데 인클루드가 안되어있는거라서 인클루드 시켜주는 작업이라고 생각하면 된다.

이제 다음으로 해야하는것은 어플리케이션을 initialize해주어야한다. 그래서 admin상태를 initalizeApp()이라고 하고 admin 옵션을 추가해주기로 한다.#

~~~javascript
admin.initializeApp(functions.config().firebase);
~~~





## 2단계(function exports)

---

이제, 우리가 만든 모든 함수들을 밖에서 사용할 수 있도록 export해주도록 하자. sendNotification() 이라는 함수를 exports해서 쓰고 싶은데, 이 함수의 역할은 notification내용에 새로운게 있는지 확인해주는 함수이다. 그걸 알수있는 방법은 

~~~
exports.sendNotifications = functions.database.ref('/notifications/{notificationId}').onWrite((event)=>{

});
~~~

이렇게 확인을 해주는것인데 database에서 notifications를 확인하고 싶지만, 모든 notification을 확인하고 싶은게 아니라 특정 noti를 확인하고 싶은것이기 때문에 {notifications}를 추가해준다. 그리고 .onWrite 함수를 사용하는데, onWrite 함수는 그 특정한 noti에 새로운 내용이 적혔는지 아닌지 확인을 해준다. #

그래서 우리는 이 sendNotifications 함수를 통해서 특정noti에 새로운 내용이 있는지 확인을 하고 그 뒤에 다른 일들을 시작할 수 있다. #

어쨌든, 나는 단지 이 내용이 바뀌었는지만 체크하고 싶을 뿐이지 내용이 변경, 추가, 삭제가 되었다고 해서 메세지가 가는것을 원하지 않는다

~~~
if (event.data.previous.val() ){
		return;
	}
~~~

그래서 코드에는 데이터가 변한게 없다면 아무것도 하지 않는다.는 코드를 적어준다. 그리고 만약 데이터가 삭제되었을때에도 아무것도 하지 않는다.

~~~
if( !event.data.exists() ){
 		return;
	}
~~~

드디어 이제 우리가 원했을때! noti를 보내는 코드를 적어보도록 하자

첫번째로 해야할일은 데이터를 가지고 오는것이다. 

~~~
const NOTI_SNAPSHOT = event.data;
~~~

두번째로는 object를 만들어주는것이다.

~~~
const payload =  {
		notification: {
			title : 'New msg from ${NOTI_SNAPSHOT.val().user}',
			body: NOTI_SNAPSHOT.val().message,
			icon: NOTI_SNAPSHOT.val().userProfileImg,
			click_action: 'https://${functions.config().firebase.authDomain}'
		}
	};
~~~

내용을 잘 보게 되면 ${NOTI_SNAPSHOT.val().user} 라는 내용이 있는데, $를 붙여주는건 ''$뒤로 나오는것은 변수명이다'' 라는 것을 알려주기 위함이고, NOTI_SNAPSHOT.val().message은 그냥 변수명이니 적어준것이다. 이것 변수들이 user인지 message인지 확인하는 법은 

![simplenoti9](/assets/img/simplenoti9.PNG)

에 보면 notification에 message, user, userProfileImg를 확인할 수 있다.

그리고 클릭했을때 우리가 파베 admin을 인클루드했던 것처럼 

~~~
https://${functions.config().firebase.authDomain}
~~~

라고 적어주었다.#

이제 내용은 다 적었다. 그런데 저 object들이 잘 받아지는지 console에 찍어보고 싶어서

~~~
console.info(payload);
~~~

를 사용했다. 여기서 info를 사용하는 이유는 log보다는 더 자세하게 보고싶기때문에 다른 타입의 console.log를 지정해주었다.

그래서 최종적으로는 exports.sendNotifications는

~~~
exports.sendNotifications = functions.database.ref('/notifications/{notificationId}').onWrite((event)=>{
	if (event.data.previous.val() ){
		return;
	}

	if( !event.data.exists() ){
 		return;
	}

	const NOTI_SNAPSHOT = event.data;
	const payload =  {
		notification: {
			title : 'New msg from ${NOTI_SNAPSHOT.val().user}',
			body: NOTI_SNAPSHOT.val().message,
			icon: NOTI_SNAPSHOT.val().userProfileImg,
			click_action: 'https://${functions.config().firebase.authDomain}'
		}
	};

	console.info(payload);

});
~~~

위 코드를 가진다.



## 3단계(deploy + 파이어베이스 function 콘솔)

---

이제 또 deploy를 해주어야한다. 저번에는 hosting만 했으니 이번에는 functions만 해주도록 한다

~~~
firebase deploy --only functions
~~~

디플로이가 완료되었으면 

![terminal1](/assets/img/terminal1.PNG)

파베콘솔에 가서 functions 를 클릭한다.

대시보드에 가보면 

![function](/assets/img/function.PNG)

functions가 들어와있는걸 확인할 수 있다!

그러면 옆에 설정에서 로그보기를 누르면 

![function](/assets/img/function2.PNG)

아직 아무런 로그도 없다. 왜냐면 아직 암것도 안했으니까...#

그러면 이제 뭔가를 해보자. 호스팅한 url로 들어가서  (로그인하고)

![simplenoti10](/assets/img/simplenoti10.PNG)

메세지를 보낸뒤에 functions콘솔에 들어가보면 오류가 뜬다

![error](/assets/img/error.PNG)

뭐지 싶으면서 어어어어어엄청나게 뒤졌지만 나오지 않는다. 설상가상으로 deploy를 하는데 뭐가 잘못된건지 css,js를 받아오지 못해서 갑자기 도메인이 허허벌판이 되었다. 롤백을 해도 소용없다. 어짜피 기능추가해서 deploy다시 해야하니까.... 그러더니 또 호스팅만 디플로이 할려니까 또!!!! An unexpected error occurred나오고... 돌아버리는줄알았다. npm 인스톨도 해보고 업데이트도 해보고 어쩌고 저쩌고... 간신히했는데 해결책인 딱히 없다. 계속 디버깅하고 deploy하니까 되어있다. 진짜 컴퓨터들은 알다가도 모르겠다. 설정해준게 없는데 아깐 안되고 지금은 됨. 사람바보 만드는건 한순간이다...#

여튼, 파베가 계속해서 업데이트가 되어서 기능들도 계속해서 추가, 삭제가 되고있는 모양인데, [Cloud 함수용 Firebase SDK 이전 가이드: 베타에서 버전 1.0으로 이전](https://firebase.google.com/docs/functions/beta-v1-diff#changes_by_trigger_type) 에서 우선 npm 설치 다 해주고, firebase-tools도 다시 설치해주고, (functions폴더안에서 설치!) 내용이 바뀌었다.

~~~
const functions = require('firebase-functions');
const admin = require('firebase-admin');
admin.initializeApp();

exports.sendNotifications = functions.database.ref('/notifications/{notificationId}').onWrite((change, context) => {

  // Exit if data already created
  if (change.before.val()) {
    return null;
  }

  // Exit when the data is deleted
  if (!change.before.exists()) {
    return null;
  }
});
~~~

초기화해주는 함수랑, 데이터를 받는 내용이 변경되었다. 그래서 계속 function콘솔에서 previous를 못읽었다고 뜨는것이었다...!#

그리고 아무일이 없어야한다고 그냥 return; 만썼었는데 return null;로 해주어야 한다. 안그러면 state는 ok로 나오지만 계속해서 에러가 뜨고 

![error](/assets/img/error1.PNG)

Function returned undefined, expected Promise or value 에러가 뜬다....

데이터도 event.data가 아니고 DataSnapshot으로 바뀌었고.... 바뀐게 넘나 많다 ㅠ_ㅠ 내가 다 찾아서 바꿨다 ㅠㅠㅠㅠㅠ#



우선은 오늘은 여기까지..

오늘 하루종일 해서 끝낼수있을거라 생각했는데 별 말도안되는것들이 문제가 되네....

