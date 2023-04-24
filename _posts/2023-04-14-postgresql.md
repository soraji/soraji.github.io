---
layout: post
title: "[ postgresQL ] postgresQL 설치와 권한부여, 그리고 DB생성, 삭제, 조회"
categories: back
comments: true
---



늘 사용해보고싶었던 postgresQL! 드디어 사용해본다.

우선 설치를 먼저해야겠지.

<br>

<br>

# postgresQL 설치

`homebrew`를 통해 다운로드 해준다

~~~
$ brew search postgresql
~~~

그러면 여러버전이 나오는데 나는 @14버전을 설치함

설치명령어는

~~~
$ brew install postgresql
~~~

설치가 완료되었다면

~~~
$ postgres --version
~~~

으로 제대로 설치되었는지 확인한다.

<br>

<br>

# postgresQL 실행

이제 설치가 다 되었다면 실행을 해주어야 한다

실행은

~~~
$ brew services start postgresql
~~~

으로 해주고 쉘에 접속하기 위해서

~~~
$ psql postgres
~~~

라고 쳐주면 postgresql 쉘에 접속한다

<br>

# 권한

접속한 뒤 사용자 및 권한에 대해 보고싶으면 `\du` 를 쳐보면 된다.

![postgresql](/assets/img/postgresql/1.png)

<br>

<br>

(근데 생성자가 처음부터 거의 대부분의 권한을 가지고 있는듯하다)

<br>

<br>

만약 어떤 user를 추가하고 비밀번호를 만들기위해서는

~~~
$ CREATE USER soraji PASSWORD 'thisisyourpassword' SUPERUSER;
~~~

이런식으로 작성해주면 된다.

<br>

<br>

권한을 주기위해서는 아래코드처럼.

~~~
$ ALTER USER soraji CREATEDB;
~~~

`CREATEDB` 권한을 soraji USER 에게 준다.

<br>

<br>

DB 소유자를 변경하여 권한을 줄때는

~~~
$ ALTER DATABASE 디비명 OWNER TO 계정명;
~~~

<br>

<br>

# DB 생성, 삭제 및 조회

DB생성을 위해서는

~~~
$ CREATE DATABASE 데이터베이스명;
~~~

<br>

<br>

목록 조회를 위해서는 

~~~
$ \list
혹은
$ \l
~~~

<br>

<br>

목록 상세조회를 위해서는

~~~
$ \list+ 
또는 
$ \lt+
~~~

<br>

<br>

DB삭제를 위해서는 

~~~
DROP DATABASE 데이터베이스명;
~~~

<br>

<br>

# 테이블 생성

이제 DB도 만들었으니 테이블을 생성해보자

~~~sql
$ create table family ( name varchar(100) not null, role varchar(100) not null);
~~~

이렇게 만들면 `CREATE TABLE` 라고 테이블이 생성된것을 확인할 수 있다

~~~sql
$ insert into family values ('세라', '막내');
~~~

이렇게 데이터를 입력해주면 `INSERT 0 1`으로 1개의 데이터가 입력된것을 확인할 수 있다

~~~sql
$ select * from family;
~~~

![postgresql](/assets/img/postgresql/2.png)

참고로 테이블 삭제는 아래와 같다.

~~~
$ drop table 테이블이름;
~~~



# 테이블 조회

`MySQL`에서는 `show tables;`로 테이블을 조회했었는데 `postgresQL`에서는 `show tables;`라는 sql이 없다.

([Postgresql tutorial](https://www.postgresqltutorial.com/postgresql-administration/postgresql-show-tables/)에 나와있음)

그래서 다른 방법으로 테이블을 확인해야한다.

우선 DB를 생성했으면 `\list` 로 DB를 조회한뒤 그 db로 들어가서 table을 조회해야한다.

그때 사용하는게 `\c` 이다.

~~~
$ \c DB이름;
~~~

그러면 psql에서 `You are now connected to database "godb" as user "soraji".` 이런cmd가 뜰거다.

그리고 나서

~~~
$ \dt;
~~~

를 쳤을때 만약 아무런 내용이 없다면 `Did not find any relations` 이런 내용이 뜬다.

내용이 있으면 테이블에 대한 내용이 나온다. (아래 이미지 있음)

<br>

<br>

~~~
$ SELECT * FROM pg_catalog.pg_tables;
~~~

이렇게 하니까 뭐가 겁나 많이 나온다;;;;; 

아니 나는 테이블을 만든적이 없는데 왜 이렇게 많이 나오는거죠;;;;;

알고 봤더니 전체 스키마 테이블이 조회되어서 그런거였다.

<br>

~~~
$ SELECT * FROM pg_catalog.pg_tables where schemaname = 'public';
~~~

그래서 where절에 조건을 붙여서 확인했더니 내가 만든것만 조회되었다

![postgresql](/assets/img/postgresql/3.png)

<br>

<br>

테이블 컬럼들에 대한 상세 내용은 [이곳](https://www.postgresql.org/docs/8.0/view-pg-tables.html)에서 확인하면 됨!

<br>

<br>

<br>

<br>

<br>

<br>



*참조 : https://kth990303.tistory.com/414*

*https://velog.io/@moonpiderman/PostgreSQL-USER-DB-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EA%B6%8C%ED%95%9C-%EB%B6%80%EC%97%AC*

*https://sas-study.tistory.com/458*























