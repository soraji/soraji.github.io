---
layout: post
title:  "팀 프로젝트 yoramyoram"
subtitle:   ""
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

![vuework](/assets/img/work/yoramyoram/main.gif)

![vuework](/assets/img/work/yoramyoram/(m)main.gif)

풀페이지로 작업했음

<br>

<br>

---

소개페이지

![vuework](/assets/img/work/yoramyoram/introduction.gif)

![vuework](/assets/img/work/yoramyoram/(m)intro.gif)

<br>

<br>

---

회원가입

![vuework](/assets/img/work/yoramyoram/signup.gif)

![vuework](/assets/img/work/yoramyoram/(m)signup.gif)

핸드폰인증(유일값)을 해야만 가입이 가능함

<br>

<br>

---

로그인페이지

![vuework](/assets/img/work/yoramyoram/login.gif)

![vuework](/assets/img/work/yoramyoram/(m)login.gif)

이메일찾기 : 이름과 핸드폰번호로 이메일을 찾을 수 있음

비밀번호 찾기 : 비밀번호 재설정으로 넘어감

<br>

<br>

---

오프라인 지도([스마트서울맵 api](https://map.seoul.go.kr/smgis2/openApi)사용)

![vuework](/assets/img/work/yoramyoram/offline.gif)

![vuework](/assets/img/work/yoramyoram/(m)offline.gif)

스마트서울맵에서 상점의 이름을 파라미터로 보내 정보를 받아오면

위도와 경도를 받아와 카카오맵과 연동시킴.

그래서 상점을 찍을때마다 지도 위치가 변경됨

<br>

<br>

---

상품목록

![vuework](/assets/img/work/yoramyoram/list.gif)

![vuework](/assets/img/work/yoramyoram/(m)list.gif)

페이지네이션으로 상품목록 조회함

<br>

<br>

상품목록에서 검색도 가능하다. TypeORM에서 `FindBy` `like` 를 사용했음

![vuework](/assets/img/work/yoramyoram/search.gif)

<br>

<br>

---

상품 상세페이지

![vuework](/assets/img/work/yoramyoram/detail.gif)

![vuework](/assets/img/work/yoramyoram/(m)detail.gif)

썸네일등록, 옵션값이 있으면 보여주고 없어주면 보여주지 않음

<br>

<br>

---

찜목록

![vuework](/assets/img/work/yoramyoram/wishlist.gif)

![vuework](/assets/img/work/yoramyoram/(m)wishlist.gif)

<br>

<br>

---

장바구니

![vuework](/assets/img/work/yoramyoram/cart.gif)

![vuework](/assets/img/work/yoramyoram/(m)cart.gif)

<br>

<br>

---

상품등록

![vuework](/assets/img/work/yoramyoram/register.gif)

![vuework](/assets/img/work/yoramyoram/(m)register.gif)

어드민 계정만 상품을 등록할 수 있다. 

옵션값은 셀렉트박스에 찍힐 `분류`와, 그 셀렉트박스 안에 옵션값으로 들어갈 `값`으로 구성되어있는데

값은 엔터를 치면 태그처럼 뜨게 만들었음





---



<br>

<br>

