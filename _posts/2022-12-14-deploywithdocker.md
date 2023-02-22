---
layout: post
title: "[ Docker ] 도커로 GCP에서 배포하기(deploy with docker)"
categories: back
comments: true
---

<br>

<br>

# GCP : deploy with docker

<br>

<br>

git에 올라가있는 프로젝트(private이면 토큰발행해야한다. 비밀번호로 인증하는건 깃에서 더이상 서비스 안함)를 우분투에 다운받는다

```
$ git clone "url.git"
```

brew update와 같은 뜻의 apt update를 해준다.

```
$ sudo apt update
```

```
$ sudo apt install docker.io
```

```
$ sudo apt install docker-compose
```

docker를 실행해야하는데 .env.docker 파일이 없기때문에 파일을 생성해야한다.

우분투에서 파일을 만드는건 `vi`, 폴더를 만드는건 `mkdir` 이다.

vim으로 파일을 만드는건데, `i ` (=insert)를 누르기전까지 입력이 되지 않는다. 입력후 esc누르면 커서가 사라짐

```
$ vi .env.docker
```

`i` 를 누르고 .env.docker 내용을 복붙한뒤 vim을 종료하기 위해서는 `shift + :` 을 눌러주면

```
 //.env.docker 내용 복붙하기

shift + :
```

제일 밑에줄에 `:` 가 생기는데 `q`는 종료, `wq` 는 저장후종료

```
$ :wq
```

도커를 빌드하고 up시켜준다.

```
$ sudo docker-compose build
```

```
$ sudo docker-compose up
```

( 근데 실제로는 db이랑 묶어서 하지 않는다.

왜냐면 도커가 잘못됐을경우 db에도 접속을 못하면 안되기때문에

GCP에 있는 데이터베이스 sql에 접속해서 앞으로 배울 것임 )

접속이 되어도 외부IP:3000번으로 접속하면 방화벽이 있기때문에 접속이 안됨

이 방화벽을 설치해줘야함

GCP에서 `VCP네트워크` 를 들어간다. 거기서 `방화벽 규칙 만들기` 들어간다

<br>

<br>

`소스 필터` : source : 출발지 ( =src ) / destination : 도착지 ( =dst )

`0.0.0.0/0` => 누구든 접속 가능(내가 0.0.0.0만 썼더니 접속 안됐음... /0도 붙여줄것...!)

`지정된 프로토콜 및 포트` : TCP체크 후 => `3000` 적어주 (3000번 포트 열어주기)

`TCP` : 안정적이다. ( '잘 받았니..?' 확인하면서 던짐 )

`UDP` : 빠름 ( '니가 받았는지 안받았는지는 내 알바아님!!!' 이러면서 막 던짐...)

<br>

<br>

VM instance 다시 들어가서 수정 누르고 네트워크 태그에 아까 방화벽에서 태그해두었던 이름을 적는다.

그리고 저장하면 방화벽 스티커가 붙은 인스턴스(=컴퓨터)가 된다.

<br>

<br>

---

# VPC 네트워크

VPC : vitual private cloud

외부 IP : 밖에서 사용하는 ip (방화벽만 열어주게되면 바로 접속이 가능함)

백엔드서버, 프엔서버는 외부ip를 열어놓으면서 VPC외부에서도 접속할수있고, 내부ip를 이용해서 서로 접속도 가능하지만

DB는 외부ip는 막아놓음(너무 위험하기 때문에)

VPC안에서만 접속이 가능하도록 만들어줌(DB는 내부ip를 이용해서 접속이 가능함).

백엔드 서버에서 내부ip를 이용해서 접속이 가능함

<br>

<br>

<br>

<br>

---

# 우분투 shell

foreground에서 실행시키기

```
sudo docker-compose up
```

background에서 실행시키기

```
sudo docker-compose up -b
```

잘 실행되는지 확인

```
sudo docker ps
```

나는 로그를 보고싶다!

```
sudo docker-compose logs
```

나는 로그가 계속해서 실행되는걸 보고싶다! (커서까지)

```
sudo docker-compose logs -f
```

로그가 너무 길어서 마지막 100줄만 보고싶다!!

```
sudo docker-compose logs -f --tail=100
```

백그라운드로 실행되는 도커 꺼주기!!

```
sudo docker-compose stop
```

<br>

<br>

<br>

```
$ cat /etc/group

///docker:x:122: 실행중인 프로그램이 뜨는데 docker를 조작하는권한을 가진 사람이 없다는 뜻
```

```
$ sudo usermod -aG docker wlthfk0211
//usermod -addGroup

$ cat /etc/group
/////docker:x:122:wlthfk0211	//계정추가함. shell한번 껐다 키고 난 이후로는 sudo안붙여도 됨
```

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>
