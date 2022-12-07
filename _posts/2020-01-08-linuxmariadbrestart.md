---
layout: post
title:  "리눅스에서 mariadb(mysql)에러 Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock'"
categories: error
comments: true


---





Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' 라는 에러가 떴다.

난 분명히... 서버를 잠깐 끄고 다시 킨거밖에없는데

분명 접속잘되던 mysql이 접속이 안된다는거시다...

찾아봤더니 경로가 달라서 그런거라 경로 설정을 해주면 된다고했는데 안된다고...ㅠㅠ아무것도 안뜬다고..



내가 설치했던 방법 고대로 다시 보니까

서버를 한번 끄면서 mysql까지 같이 꺼진것....

~~~
$ systemctl status mariadb
~~~

로 mariadb가 활성화인지 아닌지 확인하자...

inactive면 백날 접속시도해도 에러난다

만약 inactive가 되어있다면

~~~
$ systemctl start mariadb
~~~

로 살려주고, 다시 

~~~
$ systemctl status mariadb
~~~

을 명령해 확인해본다. 이러면 활성화가 된것을 볼 수 있고, 똑같이 

~~~
mysql -h localhost -u root -p
~~~

로 접속해준다.



들어가서 내가 사용하려고 하는 db이름으로 위치 이동해준다

~~~
$ use 데이터베이스명;
~~~