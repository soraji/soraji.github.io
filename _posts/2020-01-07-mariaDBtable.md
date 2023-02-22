---
layout: post
title: "[ DB ] mariaDB에 database, table생성 및 삭제하기"
categories: back
comments: true
---

## mariadb 에서 데이터베이스, 테이블 생성 및 삭제

우선 기본으로 mariadb를 사용하면,

디폴트 데이터베이스를 확인할 수 있다.

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
```

우선, RDBMS에서는 DATABASE안에 테이블들을 만드는데,

그럴려면 데이터베이스를 가장 먼저 크게 만들어주어야했다.

처음에는 바로 테이블을 만드는줄 알고

```
create database customer;
```

을 해버렸다...ㅎㅎㅎㅎㅎㅎㅎㅎㅎ 삭제하는 방법은

```
drop database `customer`;
```

이다. 작은따옴표(')가 아닌 백틱(`)이다.

### 데이터베이스 생성

---

데이터베이스의 이름은 catdog으로 했다.

```
create database catdog;
```

```
use catdog;
```

catdog라는 이름의 데이터베이스를 사용한다는 뜻이다. 이제 MariaDB shell에서 사용자의 이름이 [None]에서

[catdog]으로 바뀌었다.

catdog데이터베이스 안에서 이제는 테이블을 만들어야할 차례.

cutomer라는 이름의 테이블을 만들어보자.

```
create table customer (id varchar(100));
```

테이블을 만들때는 무조건 1개이상의 컬럼을 같이 지정해주어야 한다.

안그럼 에러뜸

id라는 컬럼을 varchar(100)으로 만들어주었다.

그리고

```
show tables;
```

하면

```
>>
+------------------+
| Tables_in_catdog |
+------------------+
| customer         |
+------------------+
1 row in set (0.00 sec)

```

요렇게 결과값이 뜬다.

다음에는 컬럼추가, insert, delete, update에 대해서 알아보쟝
