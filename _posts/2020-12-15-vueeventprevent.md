---
layout: post
title:  "[Vue.js] Vue에서 enter로 key up 될때 submit 방지"
categories: front 
comments: true

---





`@keyup.enter="함수명"` 을 하게 되면 엔터를 눌렀을때 함수명이 실행된다.

근데 문제는 엔터를 누르면 전체가 새로고침이 되어버린다. submit이 동시에 먹어버리기 떄문..

submit이 되어버리면 페이지 전체가 리로드가 되면서 내가 검색하려고 했던 모든것들이 새로고침 되버린다

보통은 keyup은 input에 걸기땜시..

input에서 

`v-on:keydown.enter.prevent='함수명'` 으로 코딩하면 submit은 방지하고 검색기능만 잘 작동한다!





사실 되게 간단한건데,

수업듣고 event.prevent~~~ 썼을때는 안먹었는데 이렇게하니까 되네!

아마 버전에 따라 달라지나 싶다













