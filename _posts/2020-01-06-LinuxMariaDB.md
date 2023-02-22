---
layout: post
title: "[ Linux ] Linux centOS에 mariaDB설치하기"
categories: back
comments: true
---

## 리눅스에 mariadb설치

사이드프로젝트를 진행하고 있는데 백이 없으니 진행이 안되서 더이상 지체할 수 없어 클라우드 구매하고 리눅스로

OS를 설치했다.

새로운 sql로 완전 혼자 처음부터 하는건 처음이라 모르는게 많아 구글신님의 도움을 받아 열심히 찾는중이다..!

리눅스로 하는건 처음이라 명령어도 익숙하지 않고, 특히나 더더더더더더더더더욱이 gui가 없어 완전 cli로만 해야하는데

오.. 생각보다 익숙하지않다.....ㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎ

우선, 가장 기본적인 centOS를 설치하고, mariadb를 설치했다.

db의 후보는 1.mssql 2.mysql 3.mongodb 4.mariadb

나는 현재 mssql을 사용하고 있지만 mssql의 엄청난 라이센스비를 내고싶지는 않고..

mysql은 커뮤니티버전을 쓰면 무료이니 사람들이 많이 사용하는데,

오라클에 인수된뒤 지금당장 오픈소스를 철수해도 이상할게 없어보인다고 해서 우선 고려했고,

mongodb는 NoSQL이지만(반드시 rdbms일필요는 없어서) 0.5GB까지만 무료라고 해서 탈락...

mariadb는 이름은 많이 들어봤는데, 알기전에는 완전 다른 db인줄 알았는데, 알고보니 mysql을 만든 개발자가

mysql이 오픈소스 라인을 타지 않는 오라클에 인수되자 반발하며 둘째딸의 이름을 따 만든것이 mariadb라고 한다.(왜 둘째딸일까...궁금쓰)

그래서 mysql과 명령어가 99%일치하고, 호환도 거의 완벽하게 된다고 한다.

우선 뭐.. 내 개인적인 생각으로는 mssql이나 mysql이나 대표적인 rdbms이기 때문에

사용법이 비슷하다고 하는 사람들이 많은데 mariadb는 거의mysql이라고 생각해도 무방하니

시작하는데 진입장벽이 높지는 않겠다.. 싶어 선택한 mariaDB이다.

너무 어렵지 않길.....바라면서 시작했다.

로그인한다(login as...)

```
$ yum install -y mariadb-server
```

ㄴ명령어로 마리아 디비를 설치한다.

`yum : Yellowdog Updater Modified 의 약자로, RPM 기반의 시스템을 위한 자동 업데이터이자 소프트웨어와 같은 패키지 설치/ 삭제 도구`

```
$ systemctl status mariadb
```

ㄴsystemctl로 mariadb가 살아있는지 확인한다.

`systemctl 이란 리눅스의 서비스 유닛들(.service로 끝나는 파일들)을 제어하는 명령어`

active가 inactive(dead)로 되어있다면 살려서 활성화시켜주어야 한다.

```
$ systemctl start mariadb
```

로 활성화 해준뒤, 다시 `systemctl status mariadb` 로 상태를 확인해보면 active가 active로 되면서 사용중임을 알 수 있다.

---

```
$  which mysql_secure_installation
>>/usr/bin/mysql_secure_installation
```

`which : 특정명령어의 위치를 찾아주는 명령어`

그리고 실행시켜준다

```
$ mysql_secure_installation
>>OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n]

```

`mysql_secure_installation`은 mariaDB를 설치할때 실행되어야하는 스크립트다

root password를 설정할것인지 물어보는 질문을 시작으로 총 4번의 질문을 하는데, 전부 y를 눌러주면 된다.

---

mysql과 연동하기

```
$ mysql -h localhost -u root -p
>>password:
$ 비밀번호입력
>>
```

로그인을 하게 되면 mysql shell로 들어가게 된다.

거기서

```
$ show databases;
```

이렇게 명령하면 데이터베이스를 보여주고, 테이블에서 select문을 실행할수 있다.

```
>>
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+

```

그럼 이런식으로 결과값이 출력된다.

앞으로 이제 가지고 놀아보자!

참고

- [Linux\] 리눅스 centOS - systemctl과 service 명령어 차이점](https://heni.tistory.com/22)
- [리눅스 which, whereis, locate - 명령어의 경로 확인](https://webdir.tistory.com/158)
