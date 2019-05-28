---
layout: post
title:  "PWA 푸시 알람 firebase 실전실습 [3편]"
subtitle:   "firebase"
categories: web
comments: true
---



여태까지 예제를 통해서 

1. [웹앱에서 푸시알림](https://soraji.github.io/developer/2018/08/09/pushmsg.html)
2. [파베를 통해서 푸시알림](https://soraji.github.io/developer/2018/08/09/firebase.html)

예제를 했기때문에 대강.. 아주대강..! 큰 틀은 이해를 하겠는데, 이걸 실전에서 어떻게 써먹을 것인가!!! 아주아주 큰 문제였다. 

우선 여태까지의 예제들은 기본 코드들을 다 다운받아서 연습해본거라 초급자에게 맞추어져있는거고, 그대로 따라만 하면 별문제없이(?) 되기때문에 별로 걱정을 안했는데, 실전은 ... 아... 어쩌나 싶었는데, PWA수업을 들으면서 유르마무님을 쉴새없이 괴롭혔다. 다시한번 죄송하다는 말씀을...ㅎㅎㅎㅎ 유림님께서 말씀하신부분은 모든 페이지에 공통으로 들어가는 페이지에다가(보통은 header가 될 듯 싶다.) 파베를 넣어주면 된다고 말씀하셨다!

나같은경우는 프레임워크로 만든게 아니기때문에 header도 따로 없고 몇달전까지만해도 공통으로 들어가는 페이지가 없었는데,!! 몇 달전 무슨 연고인지 기억은 안나지만 통일을 시켜야겠다는 생각해 모든 페이지에다 header페이지를 다 인클루드 시켰고, 그 결과 모든 페이지에 header가 들어가있는 구조로 완성되었다. 



그래서, 서비스워커(이하 sw)나 파이어베이스같은 경우에 header에 넣어주면 되어서, sw같은 경우는 예전부터 설치해놨었다. 파베도 예전부터 설치해서 콘솔에 항상 토큰값이 나오도록 했고!



(근데 이 sw랑 파베sw랑 또 다른것 같다.)

생각보다 실전에다 했는데 간단했음!

header.asp에다가

~~~javascript
<script src="https://www.gstatic.com/firebasejs/4.12.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/4.12.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/4.12.1/firebase-database.js"></script>
<script src="https://www.gstatic.com/firebasejs/4.12.1/firebase-firestore.js"></script>
<script src="https://www.gstatic.com/firebasejs/4.12.1/firebase-messaging.js"></script>
<script src="https://www.gstatic.com/firebasejs/4.12.1/firebase-functions.js"></script>
<script src="https://www.gstatic.com/firebasejs/5.3.0/firebase.js"></script>
<script>
  // Initialize Firebase
  var config = {
    apiKey: "AIzaSyDl_VS7TrZf4lyNv-7Rjshgze0dVl6vSxc",
    authDomain: "push-web-app-52e8d.firebaseapp.com",
    databaseURL: "https://push-web-app-52e8d.firebaseio.com",
    projectId: "push-web-app-52e8d",
    storageBucket: "push-web-app-52e8d.appspot.com",
    messagingSenderId: "696758097282"
  };
  var defaultApp = firebase.initializeApp(config);

console.log(defaultApp.name);
var defaultStorage = defaultApp.storage();
var defaultDatabase = defaultApp.database();

const messaging = firebase.messaging();
const tokenDivId = 'token_div';
const permissionDivId = 'permission_div';

messaging.requestPermission()
.then(function() {
  console.log('Notification permission granted.');
  return messaging.getToken();

})
.then(function(currentToken){
            console.log(currentToken)
})
.catch(function(err) {
  console.log('Unable to get permission to notify.', err);
});
</script>
~~~

파베를 cdn으로 연결한다. 그리고 파베에서 웹앱에 추가snippet을 복사해서 넣고, 초기화시킨다. (initializeApp()함수)

특히

~~~javascript
messaging.requestPermission()
.then(function() {
  console.log('Notification permission granted.');
  return messaging.getToken();

})
.then(function(currentToken){
            console.log(currentToken)
})
.catch(function(err) {
  console.log('Unable to get permission to notify.', err);
});
~~~

이 코드가 직접적으로 토큰값을 받아오는건데, 알림이 허용으로 되어있으면 콘솔에  Notification permission granted. 라고 뜨고 currentToken을 콘솔에 찍는다. 그러면 난수가 콘솔에 찍히는데 (가끔 세미콜론이 들어간 토큰도 있다. 몰라서 :이후부터 복사했는데 오류나서 그냥 처음부터 끝까지 다 복사해야함)



토큰값을 받았으면 [파베토큰값을 이용해서 메세지를 보냈던 것](https://soraji.github.io/developer/2018/08/09/firebase.html)처럼 cmd에서 (맥은 터미널) 

~~~
fcm send --server-key 파베서버키 --to 토큰키 --notification.title hi --notification.body hello
~~~

를 보냈는데, 오오!!!!

콘솔에 

Message received. {from: "696758097282", notification: {…}, collapse_key: "do_not_collapse"} 라고 떴다.

내 다시한번 말하지만, 옆에 알림이 뜨는게 중하지 콘솔에 뜨는건 중하지 않다....!!!

그래서 분명히 내가 설정을 했는데, 왜 아무것도 안뜨는것인가..! 하고 sw를 뒤져보다가, 등록되어있는 서비스 워커 unregister을 눌렀다(sw는 캐시의 역할도 하기 때문에 등록해제를 시켜주지 않으면 계속해서 내용이 남기때문에 unregister을 해주어야한다!)

unregister을 하고 메인페이지로와서 다시 강력새로고침(윈도우:ctrl+shift+R/맥:cmd+shift+R)을 눌렀더니 다시 sw가 떴다! 아 그리고 내가 메인페이지에서 걸어놓은 sw는 sw.js였는데, 파베클라우드메세지를 위핸 sw는 firebase-messaging-sw.js 여서, 이 js를 수정해주어야 했다. 분명히 내가 아는 firebase-messaging-sw.js에다가 

~~~javascript
self.addEventListener('push', function(event) {
  console.log('[Service Worker] Push Received.');
  console.log(`[Service Worker] Push had this data: "${event.data.text()}"`);

  const title = 'congrats!';
  const options = {
    body: 'this is push msg!',
    icon: '/image/icons/icon-512x512.png'
  };
const notificationPromise = self.registration.showNotification(title, options);
event.waitUntil(notificationPromise);
});

self.addEventListener('notificationclick', function(event) {
  console.log('[Service Worker] Notification click Received.');

  event.notification.close();

  event.waitUntil(
    clients.openWindow('https://mhome2.pro1.co.kr')
  );
});
~~~

이 코드를 넣었는데 왜 반응이 없는것이냐...!

그래서 js옆에 마우스를 두고 경로를 봤는데 왜 루트에 있는거지? 봤더니 예전에 예제하다가 올라간 js가 고대로 있어서 그게 작동하고 있었던거였음..! 로컬에서는 삭제했어서 더 찾기 어려웠다 ㅠ_ㅠ 결국은 찾았으니, js에 위 코드를 추가하고 다시 cmd에서 fcm명령을 하니 콘솔에도 뜨고, 바로 옆에 브라우저에서도 뜬다! 

![noti](/assets/img/pushnoti3.PNG)

(사진은 재사용...ㅋㅋㅋㅋlocalhost가 아니고 보낸 웹페이지가 뜬다)

우헿 누르면 바로 실서비스 페이지로 넘어가고! 작동이 완벽하게 잘 되었다=) 



하루종일 매달리다가 그게 딱! 되는 그 시점에 절대 놓치지 말고 사진이든 동영상이든 찍어놓을것.... 그렇지 않으면 분명히 후회하게 될것이야

근데 그 느낌이 너무 좋아서 이거 어떡하지 ㅠㅠㅠ 하루종일 매달렸는데 딱 해결되는 그 순간의 그 쾌감.... 너무 좋다!!!!