---
layout: post
title:  "firebase를 이용한 푸시알림 [2편] - 파이어베이스 Authentication(인증)"
subtitle:   "firebase"
categories: Developer
tags: firebase
comments: true
---

[제가 편집한 깃허브](https://github.com/soraji/firebase-noti.git) 에 들어가면 코드있습니당 =)

# Firebase Authentication

푸시알림을 보내려면 인증이 된 사람만 보낼 수 있게 인증처리를 해야하는데, 파베에서 굉장히 쉽게 처리할 수 있다. 특히 ''구글로 인증'' 이라던지, 많은 다른 인증수단으로 인증을 하면, 쉽게 처리 할 수 있다.



콘솔에서 [개발]-[Authentication]-[로그인방법]에 들어가서 본인이 원하는 방법을 클릭한뒤 사용설정을 활성화 시키면 된다. 나는 구글로  인증하는법, 이메일/비밀번호로 인증하는 법을 사용할것이다. 그래서 둘다 사용 설정됨 이라고 활성화시켰다.

![auth](/assets/img/auth.PNG)



이메일과 비밀번호로 사용자를 인증하는것은, 사용자탭에 들어가서 사용자추가를 누르면 된다. 

![auth](/assets/img/auth2.PNG)

사용자추가를 누르고 이메일과 비밀번호를 입력하면 로그인이 된다. 구글로 인증은 그냥 사용하면 됨!



## 1단계

---

**index.html**에 </header> 밑에

~~~html
<button id="sign-in">SignIn</button>
~~~

위 코드를 추가한다.

(아, main.js의 디폴트코드는

~~~javascript
{

/* ========================
  Variables
======================== */

const FIREBASE_AUTH = firebase.auth();
const FIREBASE_MESSAGING = firebase.messaging();
const FIREBASE_DATABASE = firebase.database();

/* ========================
  Event Listeners
======================== */

/* ========================
  Functions
======================== */
}
~~~

이다. 코드를 쳐보고 싶으면 위 코드복사한뒤에 이제부터 추가하면된다.)

그리고 **main.js**에 

~~~javascript
const signInButton = document.getElementById('sign-in');
~~~

코드를 추가한다. 

그리고 functions주석 밑에다

~~~javascript
function signIn() {
  FIREBASE_AUTH.signInWithPopup(new firebase.auth.GoogleAuthProvider());
}
~~~

을 넣어준다. 이 말인즉 firebase.auth()를 이용하는데, sign in버튼을 누르면 팝업이 떠서 인증할 수 있게끔 설정하는 function이다. 



그리고 EventListener 주석 밑에다가

~~~javascript
signInButton.addEventListener("click", signIn);
~~~

을 추가한다. signinbutton을 클릭하면 signin 이벤트가 실행!!

그러면 팝업으로 

![simplenoti2](/assets/img/simplenoti2.PNG)

구글 로그인계정이 뜬다!(처음에 보고 정말 진짜 신기했다 ㅎㅎㅎㅎ)





## 2단계

---

근데 문제는 내가 로그인이 된건지 안된건지 알수가 없는것...! 아직 아무작업도 안해줬으니 말이다..콘솔에 뭐라도 찍어야지

**main.js** EventListner주석에

~~~javascript
FIREBASE_AUTH.onAuthStateChanged(handleAuthStateChanged);
~~~

위 코드를 추가한다. 

Fuctions주석밑에는

~~~
function handleAuthStateChanged(user){
	console.log(user);
}
~~~

위 코드를 추가한다.

그리고 다시 로컬로 가서 새로고침을 누르면, 콘솔에 

~~~
S {J: Array(0), m: "AIzaSyDl_VS7TrZf4lyNv-7Rjshgze0dVl6vSxc", w: "[DEFAULT]", B: "push-web-app-52e8d.firebaseapp.com", g: R, …}
~~~

라면서 인증받은 유저의 상세정보가 뜬다.

![simplenoti3](/assets/img/simplenoti3.PNG)

이렇게 유저의 이름(displayName, email)이 뜬다. 그러면 인증을 받았다는것을 알 수 있다!!

## 3단계

---

그러면 이제 좀 더 간단하게 로그인 전후를 구별할수있게 console.log를 조금만 수정해보자. handleAuthStateChanged() 함수를 로그인후이면 사용자정보를, 로그인전이면 you haven't logged in을 콘솔에 찍어보자. 

**main.js** 에서 handleAuthStateChanged()함수를 수정해보자.

```
function handleAuthStateChanged(user){
	if(user){
		console.log(user);
	}else{
		console.log("no user");
	}
}
```



근데 아직 signOut 버튼이 없으니 로그아웃을 할수가 없으니 만들어보기로..!

**index.html**에 

~~~
<button id="sign-out">Sign Out</button>
~~~

**main.js**  전체코드(한줄씩 넣다보니 너무 귀찮....)

~~~
{

/* ========================
  Variables
======================== */

const FIREBASE_AUTH = firebase.auth();
const FIREBASE_MESSAGING = firebase.messaging();
const FIREBASE_DATABASE = firebase.database();
const signInButton = document.getElementById('sign-in');
const signOutButton = document.getElementById('sign-out');

/* ========================
  Event Listeners
======================== */
signInButton.addEventListener("click", signIn);
signOutButton.addEventListener("click", signOut);
FIREBASE_AUTH.onAuthStateChanged(handleAuthStateChanged);
/* ========================
  Functions
======================== */
function signIn() {
  FIREBASE_AUTH.signInWithPopup(new firebase.auth.GoogleAuthProvider());
}

function signOut(){
	FIREBASE_AUTH.signOut();
}

function handleAuthStateChanged(user){
	if(user){
		console.log(user);
	}else{
		console.log("you havn't logged in");
	}
}

}
~~~

그리고 나서 다시 로컬로 돌아간뒤에는 signIn/signOut 버튼이 생겨있을텐데, signOut버튼을 누르면 콘솔에 no user 가 찍힌다

![simplenoti4](/assets/img/simplenoti4.PNG)





## 4단계

---

그런데 Sign-In Sign-Out을 동시에 보여줄 필요는 없으니까 로그인전에는 Sign-In만, 로그인 후에는 Sign-Out만 보여주고싶다. 그래야지 사용자입장에서 본인이 어떤 상태인지 인지하는게 쉬울테니까!

그러기 위해서는 아까 **index.html** 에서 button태그에 

~~~html
<button id="sign-in">Sign In</button>
<button id="sign-out" hidden>Sign Out</button>
~~~

hidden을 붙인다. 그리고 로컬에서 확인하면 Sign-Out 버튼이 숨겨졌다. 이제는 사용자의 상태에 따라 버튼이 바꾸어서 보이는 작업을 해보자.

**main.js**에서 handleAuthStateChanged() 함수를

~~~javascript
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
}
~~~

이렇게 바꿔준다. 그리고 로그인하면 sign-out으로, 로그아웃하면 sign-in으로 버튼이 바뀌는것을 볼 수 있다.











다음글은 알림허용(구독)하기 입니다=)



