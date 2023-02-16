---
layout: post
title: "[ 개발 ] 백 : NestJS, 프론트 : React를 이용한 웹 어플리케이션 GIF"
subtitle: ""
categories: work
comments: true
---

`github Actions` 를 이용한 `CI`

![vuework](/assets/img/work/yoramyoram/ci.gif)

`master branch`에 `풀리퀘`가 들어오면 테스트코드가 실행된다

![vuework](/assets/img/work/yoramyoram/cd.gif)

CI 통과유무를 확인한뒤 `master branch`에 merge가 되면 CD가 실행된다

<br>

<br>

<br>

---

{ FrontEnd }

반응형으로 만들었기때문에 웹, 모바일 버전이 있음

<br>

---

메인페이지

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/main.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)main.gif" style="height:300px">
</div>

풀페이지로 작업했음

<br>

<br>

---

소개페이지

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/introduction.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)intro.gif" style="height:300px">
</div>

<br>

<br>

---

회원가입

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/signup.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)signup.gif" style="height:300px">
</div>

핸드폰인증(유일값)을 해야만 가입이 가능함

<br>

<br>

---

로그인페이지

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/login.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)login.gif" style="height:300px">
</div>

이메일찾기 : 이름과 핸드폰번호로 이메일을 찾을 수 있음

비밀번호 찾기 : 비밀번호 재설정으로 넘어감

<br>

<br>

---

오프라인 지도([스마트서울맵 api](https://map.seoul.go.kr/smgis2/openApi)사용)

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/offline.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)offline.gif" style="height:300px">
</div>

스마트서울맵에서 상점의 이름을 파라미터로 보내 정보를 받아오면

위도와 경도를 받아와 카카오맵과 연동시킴.

그래서 상점을 찍을때마다 지도 위치가 변경됨

<br>

<br>

---

상품목록

<div style="display:flex;flex-direction:row;justify-content : space-between">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/list.gif" style="width:600px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)list.gif" style="height:400px">
</div>

페이지네이션으로 상품목록 조회함

<br>

<br>

상품목록에서 검색도 가능하다. TypeORM에서 `FindBy` `like` 를 사용했음

![vuework](/assets/img/work/yoramyoram/search.gif)

<br>

<br>

---

상품 상세페이지

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/detail.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)detail.gif" style="height:300px">
</div>

썸네일등록, 옵션값이 있으면 보여주고 없어주면 보여주지 않음

<br>

<br>

---

찜목록

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/wishlist.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)wishlist.gif" style="height:300px">
</div>

<br>

<br>

---

장바구니

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/cart.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)cart.gif" style="height:300px">
</div>

<br>

<br>

---

상품등록

<div style="display:flex;flex-direction:row;justify-content : space-between;align-items:center">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/register.gif" style="height:300px">
  <img src="https://soraji.github.io/assets/img/work/yoramyoram/(m)register.gif" style="height:300px">
</div>

어드민 계정만 상품을 등록할 수 있다.

옵션값은 셀렉트박스에 찍힐 `분류`와, 그 셀렉트박스 안에 옵션값으로 들어갈 `값`으로 구성되어있는데

값은 엔터를 치면 태그처럼 뜨게 만들었음

---

<br>

<br>
