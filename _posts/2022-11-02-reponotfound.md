---
layout: post
title:  "웹 통신에 대해 알아보쟈"
subtitle:   ""
categories: web
comments: true
---



<br>

<br>

<br>

<br>

<br>

### 통신 종류

파일 통신할때 : FTP (진짜 많이 썼던 FTP..)

간단한메일 주고받는 통신 : SMTP

텍스트/하이퍼텍스트 통신 : HTTP

<br>

---

### HTTP

요청(request)/응답(response)으로 통신함

#### 상태코드

* 2xx : 성공

* 4xx : 클라이언트 오류

* 5xx : 서버오류

<br>

<br>

---

## REST API

* 주소처럼 생긴 이름

  ~~~
  https://naver.com/board/1
  ~~~

* axios

  ~~~
  import axios from 'axios'
  
  const reuslt = axios.post('url')
  ~~~

* rest api는 전체 다 받아옴(postman 이용)

<br>

---

## GRAPHQL

* 페이스북에서 만듦

* rest api 사용했더니 비효율적이라서 만들게됨

* 일반 함수와 같은 이름
  ~~~
  board(1)
  ~~~

* useMutation, useQuery
  ~~~
  import { useMutation, useQuery } from '@apollo/client'
  const result = useMutation('api이름')
  const result = useQuery('api이름')
  ~~~

* 선택적으로 값을 받아올 수 있음(rest는 다 받아옴)
  ~~~
  //받아오려고 하는 값:_id, number, message
  mutation{
    createProfile(name: "스누피", age:5, school:"댕댕이초등학교"){
      _id
      number
      message
    }
  }
  
  
  //
  //{
  //  "data": {
  //    "createProfile": {
  //      "_id": null,
  //      "number": null,
  //      "message": "프로필이 정상적으로 등록되었습니다."
  //    }
  //  }
  //}
  ~~~

  

---

## CRUD

### RestAPI 에서의 crud

* post method: 생성(create)
* put method: 수정(update)
* delete method: 삭제(delete)
* get method: 조회(read)

### apollo-client(Graphql)에서의 crud

* mutation : 생성, 수정, 삭제 (변경 = 주의요망)
* query : 조회 (변경안됨 = 막써도됨)



<br>

<br>

<br>

---

## postman

https://koreanjson.com/ 에 들어가면(한국어 무료 json은 많이 없다고 한다) 한국어 데이터를 받을 수 있다
