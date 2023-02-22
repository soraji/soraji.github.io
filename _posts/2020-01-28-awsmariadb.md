---
layout: post
title: "[ OS ] AWS EC2 우분투(ubuntu)에서 mariaDB(mysql) 설치하기"
categories: back
comments: true
---

```
$ yum install -y mariadb-server
```

이라고 치니,

''지금 yum안깔려있는데 'sudo apt install yum' 으로 깔 수 있는데?" 라고 하길래

```
$ sudi apt install yum
```

으로 우선 yum설치를 하고,

```
$ yum install mariadb-server
```

를 하니,

```
>> There are no enabled repos.~~~
```

라면서 레포가 없다고 떴다.

[Install MariaDB 10.3 on Ubuntu 16.04 LTS](https://computingforgeeks.com/how-to-install-mariadb-10-3-on-ubuntu-16-04-lts-xenial/)

우분투 16.04에 마리아디비를 설치하려면 레포지토리가 있어야한다고 한다.

레포를 만드는건

```
sudo apt -y install software-properties-common dirmngr
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
sudo add-apt-repository 'deb [arch=amd64] http://mirror.zol.co.zw/mariadb/repo/10.3/ubuntu xenial main'
```

```
sudo apt update
sudo apt -y install mariadb-server mariadb-client
```

이렇게 설치를 하면 설치가 되다가 중간에 gui가 뜨면서 root비번을 설정하라는 창이 뜬다.

그럼 비번쳐주면 됨. 앞으로 이게 root의 비번이 될것임

그리고

```
$ mysql -u root -p
```

이라고 명령하면 비번을 치라고 하는데, 비번을 쳤더니 이제는

ERROR 1524 (HY000): Plugin 'unix_socket' is not loaded

이런 에러가 뜬다.

[ERROR 1524 (HY000): Plugin 'unix_socket' is not loaded](https://wnw1005.tistory.com/57)

윗글 참고했음

```
$ sudo systemctl stop mariadb
$ sudo mysqlid_safe --skip-grant-tables &
```

이후

```
$ mysql -u root
```

라고 치니, mariaDB에 접속이 되었다.

이후 DB생성하는 것은 똑같다.

```
$ create database 데이터베이스이름;
$ use 데이터베이스이름;
```
