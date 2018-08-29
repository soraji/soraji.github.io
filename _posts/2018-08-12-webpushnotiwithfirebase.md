---
layout: post
title:  "firebase를 이용한 푸시알림 [3편] - 파이어베이스를 이용한 웹 푸시알림"
subtitle:   "firebase"
categories: Developer
tags: firebase
comments: true
---

[제가 편집한 깃허브](https://github.com/soraji/firebase-noti.git) 에 들어가면 코드있습니당 =)

## 1단계(**firebase-messaging-sw.js** 만들기)

---

루트폴더에 firebase-messaging-sw.js 파일을 만든다. 

[파베공식문서](https://firebase.google.com/docs/cloud-messaging/js/receive) 에 들어가거나 아니면 그냥 아래 코드들을 복사해서 인클루드시킨다.

~~~javascript
importScripts('/__/firebase/3.8.0/firebase-app.js');
importScripts('/__/firebase/3.8.0/firebase-messaging.js');
importScripts('/__/firebase/init.js');
~~~

그리고 아래에 

~~~javascript
firebase.messaging();
~~~

코드를 추가한다. 이 **firebase-messaging-sw.js**파일은 별다른 레퍼런스가 필요하지 않다. 

예전에 파베를 막 이것저것 해보다가 계속 콘솔에 firebase-messaging-sw.js 404라고 떠서 stackoverflow에 가보니까 아무리 빈내용이어도 파일자체가 존재해야 한다고 하더라... 그냥 빈 파일에 이름만 firebase-messaging-sw.js 인 js파일을 만들면됨. 그리고 그 sw가 fcm을 도와줄것임 =)



## 2단계(subscribe버튼 만들기)

---

이제 해야하는건, 사용자가 구독을 할 수 있도록 subscribe 버튼을 만드는 것이다. index.html에 

~~~html
<button id="subscribe">
	Subscribe
</button>
~~~

코드를 넣고

**main.js**에 Variables주석에 아래 코드

~~~javascript
const subscribeButton = document.getElementById('subscribe');
~~~

를 넣고, 이벤트리스터 주석에다가는

~~~
subscribeButton.addEventListener("click", subscribeToNotifications);
~~~

를 넣는다.

---

## 어쩌면 이게 가장 중요한 부분이다. 파베를 이용한 메세지를 보내는 가장 중요한 원리는 처음에 웹사이트를 접속했을때 '알림을 허용하겠느냐'라는  

## 1. 알림허용여부를 물어본뒤 

## 2. 사용자의 토큰값을 가져온다.

## 3. 그리고 그 토큰값을  DB 에다가 저장한다.

 ## 그래야 앞으로 메세지를 보낼때 이 레퍼런스를 이용할 수 있기 때문이다.



---

펑션주석에다가는 

```
function subscribeToNotifications() {
  FIREBASE_MESSAGING.requestPermission();
}
```

을 넣는다. 

requestPermission() 함수가 알림을 뜨게한다는데, 난 왜 아무리해도 안뜨는 것이냐..... 도대체 와이 ㅠㅠㅠㅠㅠㅠ

그래서 그냥  매뉴얼로 알림허용버튼을 누르고 

![allow](/assets/img/allow.PNG)

subscribe를 눌렀다. 아직까지는 아무것도 한게 없는거다!



## 3단계(토큰 생성)

---

이제 토큰값을 가져오기로 하자.

~~~
function subscribeToNotifications() {
  firebase.messaging().requestPermission()
    .then(() => firebase.messaging().getToken())
    .then(() => console.log(token))
    .catch((err) => {
      console.log("error getting permission :(");
    });
}
~~~

위의 subscribeToNotifications()함수에다가 내용을 추가해준다고 했는데, 내가 subscribe을 누르니까 error getting permission이 떠서, 이것저것해봤는데 토큰값이 뜨지 않았다. (.babelrc 파일이 없어서 그러는가 했는데 만들어도 여전히 안됨 ㅠㅠ) 그래서 내가 예전에 가지고 있던 코드로

~~~
function subscribeToNotifications() {
  FIREBASE_MESSAGING.requestPermission()
	.then(function() {
	  console.log('Notification permission granted.');
	  return FIREBASE_MESSAGING.getToken();
	})
	.then(function(currentToken){
	            console.log(currentToken)
	})
	.catch(function(err) {
	  console.log('Unable to get permission to notify.', err);
	});
}
~~~

이렇게 수정했고, 새로고침 후 subscribe를 눌렀더니 토큰값을 잘 가지고 온다.yay!



## 4단계(DB저장)

---

이제 토큰값을 얻었으니, DB에 저장만 하면 된다. subscribeToNotifications()함수에

~~~
FIREBASE_DATABASE.ref('currentToken').push({
	currentToken : currentToken,
	uid: FIREBASE_AUTH.currentUser.uid
});
~~~

위 코드를 추가한다. 추가하면 아래 처럼 될것이다.

~~~
function subscribeToNotifications() {
  FIREBASE_MESSAGING.requestPermission()
	.then(function() {
	  console.log('Notification permission granted.');
	  return FIREBASE_MESSAGING.getToken();
	})
	.then(function(currentToken){
        console.log(currentToken);

        FIREBASE_DATABASE.ref('currentToken').push({
        	currentToken : currentToken,
        	uid: FIREBASE_AUTH.currentUser.uid
        });
	})
	.catch(function(err) {
	  console.log('Unable to get permission to notify.', err);
	});
}
~~~

그리고 firebase.com의 콘솔로 이동해보자. 이 파베프로젝트에 들어간다. 

![db](/assets/img/db.PNG)

그러면 ref뒤에 변수로 넣은 변수명과 함께 랜덤아이디값이 뜬다. (위 사진 참조!)

그리고 + 를 눌러보면 우리가 원했던 currentToken값과 uid값을 잘 받아온것을 볼 수 있다!!! 브라우저에서 봤던 토큰값과 db에 들어간 토큰값이 일치한다!!=)

![db2](/assets/img/db2.PNG)

이렇게 저장이 되었기 때문에 이제 앞으로 이 토큰값을 ref로 원하는 기기에 push할 수 있다..!!



## 5단계(새로운 토큰값 생성 함수 만들기)

---

main.js에서 firebase auth의 상태가 바뀌었을때(로그인 전/후와 같이)

~~~
FIREBASE_AUTH.onAuthStateChanged(handleAuthStateChanged);
~~~

firebase messaging에서도 상태가 변화했을때를 확인해주어야 한다. 알림을 허용했던 사용자가 더이상 알림을 허용하지 않는다던가, 아니면 차단했던 사용자가 알림을 허용한다던가.. (솔직히 후자는 문제될게 없다. 그냥 새로운 토큰값을 받는 new 유저와 같이 때문이다.) 그런데 만약에 알림을 허용했던 사용자가 알림을 거부했다가, 다시 알림을 허용한다면???? 그때는 새로운 토큰값을 받아와야 할것이다. 그때 사용하는 이벤트리스너를 새로 만들어보자.

~~~
FIREBASE_MESSAGING.onTokenRefresh(handleTokenRefresh);
~~~

그렇다면 handleTokenRefresh() 함수를 만들어주어야 할 것이다.

~~~
function handleTokenRefresh() {
  return FIREBASE_MESSAGING.getToken()
  .then(function(currentToken){
    FIREBASE_DATABASE.ref('currentToken').push({
      currentToken: currentToken,
      uid: FIREBASE_AUTH.currentUser.uid
    });
  });
}
~~~

그런데 handleTokenRefresh() 함수의 내용이 subscribeToNotifications()의 내용과 같기때문에 subscribeToNotifications()함수 내용을 바꿔도 되지만.. 난 그냥 놔둘란다.ES6를 사용해서 하고 싶은데 아직 설정을 안해놔서 그런가 안먹는다..!



## 6단계(unsubcribe버튼 만들기 및 DB삭제)

---

이제 어느정도 코드를 넣은 사람들이라면 구조를 알테니 어느주석에다 달았는지까지는 상세하게 안적고 그냥 적어야겠다. 이거슨 투머치에포트...

~~~
const unsubscribeButton = document.getElementById('unsubscribe');
//=======================================
unsubscribeButton.addEventListener("click", unsubscribeFromNotifications);
~~~

코드를 추가해준다.

이제 unsubscribeFromNotifications() 함수를 만들어줄건데, 구독하기 버튼을 누른것과 마찬가지로 몇가지 단계가 있다. 사용자가 구독을 더이상 희망하지 않는다면, 해야할 단계로는 첫번째, 거부를 희망하는 사용자의 토큰값을 가져온다. 그리고 db에서 그 토큰값을 찾아 삭제하는 것이다.

*하다가 ES6가 작동하는것을 알아냈다.. 그래서 ES6로 작성하기루..*

~~~
function unsubscribeFromNotifications() {
  FIREBASE_MESSAGING.getToken()
    .then((token) => {
    		console.log(token)
    		FIREBASE_MESSAGING.deleteToken(token)
    })
    .then(() => FIREBASE_DATABASE.ref('currentToken').orderByChild("uid").equalTo(FIREBASE_AUTH.currentUser.uid).once('value'))
    .then((snapshot) => {
    	console.log(snapshot.val());
      const key = Object.keys(snapshot.val())[0];
      return FIREBASE_DATABASE.ref('currentToken').child(key).remove();
    })
    .catch((err) => {
      console.log("error deleting token :(");
    });
}
~~~

이 함수가 어떻게 보면 굉장히 중요하다.

그러니까 찬찬히 뜯어보기로 하자. 우선은 

~~~
FIREBASE_MESSAGING.getToken()
~~~

으로 토큰값을 가져온다. 그리고

~~~
.then((token) => {
    		console.log(token)
    		FIREBASE_MESSAGING.deleteToken(token)
    })
~~~

혹시 모르니 콘솔에 토큰값을 찍어본다. 가지고 왔으면 토큰을 삭제하라고 한다.

~~~
.then(() => FIREBASE_DATABASE.ref('currentToken').orderByChild("uid").equalTo(FIREBASE_AUTH.currentUser.uid).once('value'))
~~~

그리고 파베 DB에 저장되어있는 currentToken을 (솔직히 token 이나 currentToken이나 그냥 다 토큰값임) uid기준으로 정렬해서 파이어베이스 auth에서 currentuser.uid(FIREBASE_AUTH.currentUser.uid) 값과 일치하는 value를 가지고 있는 db를 찾는다.

~~~
.then((snapshot) => {
    	console.log(snapshot.val());
      const key = Object.keys(snapshot.val())[0];
      return FIREBASE_DATABASE.ref('currentToken').child(key).remove();
    })
~~~

그리고 저 snapshot이 정확하게 뭔지는 모르겠는데, 현재 사용자가 가지고 있는 정보를 보여준다. firbase AUTH를 이용해서 로그인하고 콘솔창에 user를 치면 user에 대한 상세정보가 나오듯이 snapshot은 상세정보를 보여준다. 그러면 그 uid 값을 기준으로 일치하는 uid를 가지고 있는 토큰값을 삭제한다. 왜냐면 지금 이 글을 쓰면서 계속 테스트하고 있는데 토큰값이 계속 바뀜....... 변동이 없는게 아니라서 고정값이 uid를 같이 비교해주어야 할 필요가 있다.



그리고 가서 subscribe를 누르면 토큰값이 뜨면서 파베콘솔에 가보면 

![db2](/assets/img/db2.PNG)

이렇게 토큰값이 뜨고, 

unsubscribe 버튼을 누르면 콘솔에 또 토큰값이 뜨면서 

![db3](/assets/img/db3.PNG)

으로 실시간으로 토큰값이 생겼다 지워졌다를 확인할 수 있다(진심 짱신기함. 파베 너무 매력적이다...) 그 말인즉, 구독/비구독 버튼을 이용해서 db에 있는 값도 생성/삭제를 할 수 있다는 말이다!!!!



## 7단계(구독/비구독 버튼 상태바꾸기 depends on your status)

---

우리가 만들었던 로그인 전=>sign in / 로그인후=>sign out 이렇게 사용자의 상태에 따라서 버튼을 숨기고 보이게 하는 걸 다시 해볼것이다.

~~~
function checkSubscription() {
  FIREBASE_DATABASE.ref('tokens').orderByChild("uid").equalTo(FIREBASE_AUTH.currentUser.uid).once('value').then((snapshot) => {
    if ( snapshot.val() ) {
      subscribeButton.setAttribute("hidden", "true");
      unsubscribeButton.removeAttribute("hidden");
    } else {
      unsubscribeButton.setAttribute("hidden", "true");
      subscribeButton.removeAttribute("hidden");
    }
  });
}
~~~

자세히 읽어보면 db에 uid값이 있으면 unsubscribe으로 바꾸고, uid값이 없으면 subscribe으로 뜨게 하는것!

index.html에는

~~~
<button id="subscribe" hidden>Subscribe</button>
<button id="unsubscribe" hidden>UnSubscribe</button>
~~~

hidden을 넣어서 디폴트로 아무것도 뜨지 않게 한다. 그리고 사용자라는게 확인이 되었을때만 구독/비구독 버튼이 뜨게 만드는것은 handleAuthStateChanged() 함수를 찾아서 

~~~
function handleAuthStateChanged(user){
	if(user){
		console.log(user);
		signInButton.setAttribute("hidden","true");
		signOutButton.removeAttribute("hidden");
	}else{
		console.log("no user");
		signOutButton.setAttribute("hidden","true");
		signInButton.removeAttribute("hidden");
	}

	checkSubscription();
}
~~~

로 바꿔준다. 이래야지 로그인을 한뒤에 유저라는게 확인되었을때checkSubscription() 함수가 실행되기 때문이다.

![simplenoti5](/assets/img/simplenoti5.PNG)

그리고 새로고침을 하면 난 현재 로그인 상태이고 아직 구독을 누르지 않았기때문에(혹은 방금 전 비구독을 눌렀기 때문에 ) subscribe 버튼만 보이는걸 볼 수 있다.



## 7단계 (구독후/비구독 후 구독여부확인하기)

---

그러니까 checkSubscription() 함수는 사용자가 구독을 했는지 안했는지를 확인하고, subscribe버튼과 unsubscribe버튼 중 어떤것을 띄워야하는지 사용자의 구독상태를 확인해주는 함수이다.  그러니까 구독버튼을 누른뒤, 비구독버튼을 누른뒤에도 작동해야 하는 함수이므로, subscribeToNotifications() 함수와 unsubscribeFromNotifications() 함수에다가 checkSubscription() 함수를 추가해주어야한다.

subscribeToNotifications()함수는 아래처럼

~~~
function subscribeToNotifications() {
  FIREBASE_MESSAGING.requestPermission()
	.then(function() {
	  console.log('Notification permission granted.');
	  return FIREBASE_MESSAGING.getToken();
	})
	.then(function(currentToken){
        console.log(currentToken);

        FIREBASE_DATABASE.ref('currentToken').push({
        	currentToken : currentToken,
        	uid: FIREBASE_AUTH.currentUser.uid
        });
	})
	.then(() => checkSubscription())
	.catch(function(err) {
	  console.log('Unable to get permission to notify.', err);
	});
}
~~~

unsubscribeFromNotifications() 함수는 아래처럼 바꿔준다.

~~~
function unsubscribeFromNotifications() {
  FIREBASE_MESSAGING.getToken()
    .then((token) => {
    		console.log(token)
    		FIREBASE_MESSAGING.deleteToken(token)
    })
    .then(() => FIREBASE_DATABASE.ref('currentToken').orderByChild("uid").equalTo(FIREBASE_AUTH.currentUser.uid).once('value'))
    .then((snapshot) => {
    	console.log(snapshot.val());
      const key = Object.keys(snapshot.val())[0];
      return FIREBASE_DATABASE.ref('currentToken').child(key).remove();
    })
    .then(() => checkSubscription())
    .catch((err) => {
      console.log("error deleting token :(");
    });
}

~~~





---

이렇게 구독과 비구독을 통해서 토큰값을 받아오고 DB에다가 저장하는 법까지 알아봤다. 이제 다음편은 파베를 이용해서 메세지를 만들고, 저장하는 법을 알아보도록 하자=) 솔직히 코드만 보면 어려워보이는데 막상해보면 원리는 굉장히 간단하다. 그러니 실습은 꼭 한번은 해봤으면 좋겠다. 솔직히 그냥 코드 복붙하고 되나안되나 확인해보면 됨 ㅎ_ㅎ 근데 이걸 해보느냐 안해보느냐의 차이가 이해하는데에 absolutely차이가 있다고 말할 수 있다. 꼭 한번 해보길!