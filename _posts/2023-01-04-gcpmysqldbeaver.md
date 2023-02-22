---
layout: post
title: "[ GCP ] GCP MySQL DBeaver 연결 / GCP MySQL cli로 연결하기(Google Cloud Shell)"
categories: back
comments: true
---

<br>

<br>

<br>

<br>

# GCP mysql cli로 연결하기

gcp에서 mysql을 사용하기로 했는데 모든게 정상 작동인데,

저장되는 데이터들을 시각적으로 보고싶은거다.

물론 cli로 볼수는 있지만.. 불편하니까.

근데 이러나저러나 알아놔야하니까 기록.

gcp에서 기본적으로 mysql을 만들면 그걸 조회해보는건 gcloud shell에서 가능한데,

이것도 로그인했다고해서 바로 되는게 아니다.

<br>

<br>

![gcp](/assets/img/2023-01-04/5.png)

이러나저러나 외부에서 접속을 하기 위해서는 공개ip를 눌러서 수정해줘야한다.

공개ip를 설정을 안해주면 클라우스쉘에서조차 접속이 불가능하다.

그대신 공개ip로 해놓으면 보안이.... 진짜 최악이니까

필요할때만 잠깐 수정했다가 다 이용했으면 다시 공개ip를 꼭 🌟끄자🌟

(이것도 우리반친구랑 얘기하다가 나온건데 멘토한테 물어보니까 쓸때만 잠깐 공개ip로 돌려서 사용하라고 그랬다고 한다.

더 좋은 방법 없나...? 접속이 힘들거같긴한데.. 클러스터 안에 있어서 외부ip는 아예 접속이 안되는건 이해하는데 좀 너무 불안..)

<br>

<br>

<br>

gcloud shell에서 접속해보자

![gcp](/assets/img/2023-01-04/1.png)

인스턴스에 접속해야하는데, 인스턴스이름이 뭔지 어떻게 알아....

그럴때 사용하는 명령어다.

```
$ gcloud sql instances list
```

(이렇게 치면 인스턴스 정보에 대해서 나오는데, 보니까 sql만들때 그 sql 이름이었다.)

```
$ gcloud sql connect 인스턴스이름 --user=root
```

![gcp](/assets/img/2023-01-04/2.png)

```
show databases;
```

데이터베이스 조회

![gcp](/assets/img/2023-01-04/3.png)

```
use yoramyoram;
```

`yoramyoram` 사용하기로 결정

```
show tables;
```

테이블 조회

![gcp](/assets/img/2023-01-04/4.png)

```
select * from user;
```

거기서 조회를 원하는 테이블에서 mysql 가지고 놀면된다.

# GCP mysql DBeaver 연결

![gcp](/assets/img/2023-01-04/6.png)

진짜 최악이지만 디비버에서 연결하려면 어쩔수없다.

`네트워크 추가` 를 눌러 `0.0.0.0/0` 을 등록해준다.

그리고 디비버에서 공개ip를 host에 넣고, 아이디, 비번을 넣으면 바로 접속된다.

만약 안된다면 방화벽 처리를 해줘야한다.

`VPC 네트워크 > 방화벽` 에서 `방화벽 규칙 만들기` 를 누르고

이름은 아무거나 (대신 `대상태그` 에는 이름을 동일하게 적어주어야한다)

로그 > 사용안함

트래픽 방향 > 수신

일치 시 작업 > 허용

소스 IPv4 > 0.0.0.0/0

TCP > 3306

이렇게 생성하면 됨

![gcp](/assets/img/2023-01-04/7.png)

제대로 연결하면 이렇게 잘 불러온걸 알수있다.

<br>

오늘도 성공..!

<br>

<br>

<br>

<br>

<br>
