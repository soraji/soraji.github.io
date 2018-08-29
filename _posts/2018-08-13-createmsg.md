---
layout: post
title:  "firebase를 이용한 푸시알림 [4편] - 메세지 생성, 저장"
subtitle:   "firebase"
categories: Developer
tags: firebase
comments: true
---

[제가 편집한 깃허브](https://github.com/soraji/firebase-noti.git) 에 들어가면 코드있습니당 =)

이제 진짜 재밌는 부분이 시작된다..! 여태까지는 메세지를 주고받을 수 있는 환경을 만들었다고 생각하면 되고, 이제 할것들은 실제로 메세지를 구성하고 보내는, 작업을 해볼거다..!!



## 1단계(form양식만들기)

---

이제 form양식을 만들어서 input내용을 받을 수 있도록 해보자.

index.html에 form태그를 추가한다.

~~~
<form id="send-notification-form">
  <label class="sr-only" for="notification-message">Notification Message</label>
     <div>
        <textarea id="notification-message" maxlength="40" placeholder="What would you like to say?"></textarea>
        <button type="submit" id="send-notification" aria-label="Send">
         <img src="img/send.png" alt="">
       </button>
     </div>
   </form>
~~~

그러면 아래 사진처럼 이미지가 폼이 나올거다.

![simplenoti6](/assets/img/simplenoti6.PNG)

그리고 main.js에

~~~
const sendNotificationForm = document.getElementById('send-notification-form');
//================================
sendNotificationForm.addEventListener("submit", sendNotification);
~~~

를 적어준다.





## 2단계(DB에 notification저장)

---

그럼 이제 sendNotification() 함수를 적어보자.

sendNotificationg() 함수는 유저가 form에다가 노티를 보내고 싶은 말을 적고, 그 내용을 파베 db로 보내는데, 새로 추가 된 내용이 있으면 noti를 보내주는 방식이라고 생각하면 된다.

~~~
function sendNotification(e) {
  e.preventDefault();

  const notificationMessage = document.getElementById('notification-message').value;
  if ( !notificationMessage ) return;

  FIREBASE_DATABASE.ref('/notifications')
    .push({
      user: FIREBASE_AUTH.currentUser.displayName,
      message: notificationMessage,
      userProfileImg: FIREBASE_AUTH.currentUser.photoURL
    })
    .then(() => {
      document.getElementById('notification-message').value = "";
    })
    .catch(() => {
      console.log("error sending notification :(")
    });
}
~~~

[저번편](https://soraji.github.io//developer/2018/08/13/webpushnotiwithfirebase.html)에서 해본것처럼, db에 내용을 넣었을때 저번에는 토큰을 넣었는데 이번에는 notification을 넣는다고 생각하면 된다. 

~~~
FIREBASE_DATABASE.ref('/notifications')
    .push({
      user: FIREBASE_AUTH.currentUser.displayName,
      message: notificationMessage,
      userProfileImg: FIREBASE_AUTH.currentUser.photoURL
    })
~~~

이 내용이 그 내용이다. 콘솔user에 보면 displayName 필드가 있다. phtoURL도 있고.. 이건 나중에 푸시알림뜰때 나올 내용들이다.





![simplenoti7](/assets/img/simplenoti7.PNG)

라고 보내면, 

![simplenoti7](/assets/img/simplenoti8.PNG)

라면서 notification이 뜨고, +를 누르면 message에는 does it work? 라고 뜬다!!!!!!!!!!!!!!! 오오미 신나는것!!!!!!!!!!

![신이난다](/assets/img/happy/1.jpg)

그러나, 계속해서 누누이 얘기했듯이, db에 저장되는것으로 충분하지 않다.

푸시알림이 떠야 이 모든것은 마무리 된다고 볼 수 있겠다.





## 3단계(deploy 단계)

---

이제 deploy를 해주어야할 차례이다. deploy가 뭔지 몰라서 찾아봤는데 deploy는 firebaseapp.com으로 된 사이트를 배포하는 명령어 이다!!  사이트를 배포하려면

~~~
firebase deploy
~~~

명령어를 cmd나 터미널에 치면된다. 공식문서에 따르면 

**참고:** **SSL 전용**: Firebase 호스팅은 SSL 전용으로서 HTTPS를 통해서만 콘텐츠를 제공합니다. 따라서 애플리케이션 개발 단계에서 외부 스크립트 등 Firebase 호스팅에서 호스팅되지 않는 모든 외부 리소스는 SSL(HTTPS)을 통해 로드되어야 합니다. 대부분의 브라우저에서는 사용자가 '혼합 콘텐츠'(SSL 트래픽과 SSL이 아닌 트래픽)를 로드할 수 없습니다. 

라고 나와있다.

우리가 맨처음에 이걸 만들었을때, function, database, hosting을 선택하고 파베프로젝트를 만들었기 때문에 아무런 설정없이 그냥 디플로이를 해주면 모든게 다 배포된다. 그러면 올리는 파일도 길어지고 많아지니, 설정을 해줄필요가 있다. 우리는 처음에 만들었을때 public폴더를 만들지 않고 루트에다가 만들었기 때문에 

**firebase.json** 파일에 들어가서 아래처럼 바꿔준다. public은 루트 경로이고 ignore 배열에 들어있는것들은 deploy되지 말라고 적어준다.

~~~
{
  "database": {
    "rules": "database.rules.json"
  },
  "hosting": {
    "public": ".",
    "ignore": [
      "firebase.json",
      "database-rules.json",
      "functions"
    ]
  }
}
~~~

그리고 

~~~
firebase deploy --only hosting
~~~

을 하는데, 왜!!! 왜ㅐㅐㅐㅐㅐㅐㅐㅐㅐㅐ!!!!!!!!! 와이!!!!!!!!!!!!!!!!!!!

계속해서 an unexpected error has occurred 하면서 deploy가 안되는거ㅠㅠㅠㅠㅠㅠ왜 안되는데 왜애ㅐㅐㅐㅐ와이ㅣㅣ와이와이ㅣㅣㅣㅣㅣㅣ 

열심히 이것저것 또 뒤지다가 

![deploy](/assets/img/deploy.PNG)

발견하고 파베 CLI 업데이트 해줬다......

그리고 디플로이 하니까 잘 됨^^^^^^

정말 개발은 코드만 잘치는게 아니고 이 설정... 이 설정 이게 정말....... 설정이 안되서 error났다고 하면 정말 어디서 어떻게 디버깅해야하는지 완전 멘붕이다...

여튼 해결됐으니까. 

![terminal](/assets/img/terminal.PNG)

아니 근데 hosting에서 ignore하라고 한거 많은데 뭔 19374 files이나 되냐.. 분명 뭐가 잘못된것이 틀림없다ㅏㅏㅏㅏㅏ

하지만 deploy는 되었으니 만족하고 이건 나중에 다시 수정해보든지 해야지 

그러면

![firefox](/assets/img/firefox.PNG)

이렇게 호스팅된 도메인이!!!!!!!!!!!! 아무데서나 URL을 치고 들어갈수있따!!! 나는 완전 다른 파폭에서 시험해봤다. 잘됨 굳굳굳!



그러면.. 내용을 한번 보내보자. 아참, 로그인먼저하고! 

![firefox](/assets/img/firefox3.PNG)

그리고 subscribe를 누르면 알림허용할건지 물어본다. 당연히 알림허용!!

그리고  still working? 이라고 보내본다.

![firefox](/assets/img/firefox2.PNG)



그리고 나서 파베콘솔에가서 확인한다.

![simplenoti9](/assets/img/simplenoti9.PNG)



그러면 저 밑에 still working?이라는 notification이 들어와있는걸 볼 수 있다







---

하...넘나 감격.... 이걸 해내다니........... 아 정말 ㅠㅠㅠㅠㅠㅠ대견스럽다 흐엉

파베로 호스팅하고, notification내용을 db에서 넘기는작업까지 해봤다.

이제 다음은, 정말 유저의 브라우저에서 푸시알림이 뜨는단계까지! 해보도록 하자=) 





