---
layout: post
title: "[ K8s ] 쿠버네티스로 배포하기 1편 (나의 NestJS 프로젝트 GCP에 올리기)"
categories: back
comments: true
---

<br>

<br>

# 쿠버네티스로 배포하기 1편

배포하다가 멘붕 온 사람으로.... 몇시간이 걸려도 기록하고 말겠다....

오늘 배포하고 오늘 안적어놓으면 분명 까먹을거니까 오늘 기록한다..... [≡] ✍(・⺫・‶)

배포 얘기하는데 완전 바보가 된 느낌... 아는게 하나도 없네..

배포는 뭔가 한번 꼬이니까 어디가 잘못된건지 알수가 없다.... 차라리 코딩이 더 쉽다....

aws, gcp, azure 들어오면 진짜 단어들이 다들 너무 어려워서 뭐가 뭔지 모르겠다...

<br>

<br>

<br>

쿠버네티스로 배포하기 이전에, 쿠버네티스가 뭔지 먼저 알아야지. (๑❛ڡ❛๑)☆

쿠버네티스는 컨테이너(도커) 관리자로 컨테이너 운영을 자동화하기 위한 도구이다.

이런 도구들을 오케스트레이션 도구(지휘자의 역할)라고 한다. (쿠버네티스 말고도 도커스웜 등 여러 도구들이 있다)

컨테이너를 이용한 어플리케이션 배포 외에도 다양한 운영관리 업무를 자동화할 수 있다.

(컨테이너 배치, 로드밸런싱, 헬스 체크 등)

![쿠버네티스](/assets/img/kubernetes/4.png)

쿠버네티스에서 중요한 개념이 바로 pods(파드)인데, 예전에는 도커 컨테이너가 중요한 개념이었다면

이 컨테이너들을 관리하는 것들이 파드라고 생각하면 된다.

> 파드란?
>
> 컨테이너가 모인 집합체의 단위.
>
> 적어도 하나 이상의 컨테이너로 이루어져있다.
>
> 파드는 하나의 노드에서만 배치가 가능하고, 다른 노드에 걸쳐서 배치될 수 없다.
>
> 즉, 하나의 컴퓨터에서만 배치가 가능하고 다른 컴퓨터와 같이 배치될 수 없다.

<br>

<br>

---

# 쿠버네티스로 배포하기

<br>

aws로 배포를 해도되지만 돈이 너무 많이 청구될 수 잇으므로 무료 크레딧을 받을 수 있는 GCP를 이용할거다.

(개념은 똑같다고 한다)

![쿠버네티스](/assets/img/kubernetes/1.png)

GCP에서 `클러스터 엔진 카테고리 > 클러스터` 로 들어간다.

`만들기` 를 만들어서 `AutoPilot` 을 선택한다.

standard로 선택하는 경우는 내가 쿠버네티스를 잘 알고있는경우,

이 클러스터를 만들면 얼마만큼의 메모리, 얼마만큼의 CPU가 필요한지 등등 사전 정보를 알고있어서

아키텍쳐를 잘 짤 수 있을때 사용하는게 좋다.

잘 모르면 그냥 오토파일럿 쓰자.. (·•᷄‎ࡇ•᷅ )

![쿠버네티스](/assets/img/kubernetes/3.png)

오토파일럿은, 위 그림에도 나와있듯이

유저의 접속량에 따라 노드의 갯수를 줄였다 늘였다 하는 기능이다.

접속자가 없으면 필요없는 리소스 낭비를 방지하기 위해 노드를 없애고,

접속자가 많으면 서버가 터지지 않게 노드를 늘린다.

<br>

<br>

### 클러스터 만들기

---

![쿠버네티스](/assets/img/kubernetes/2.png)

우선은 `클러스터`를 먼저 만들자.

이름과 리전을 선택한다. 서울은 `asia-northeast3` 이다.

그리고 `만들기` 클릭

<br>

<br>

<br>

<br>

### 클러스터에 연결할 코드들 push하기(Container Registry이용)

---

이제는 vscode에서 설정해주자.

GCP와 연결하기전에 docker-compose.prod.yaml파일을 먼저 수정해서 push해주자.

왜냐면 이 push한 코드로 배포를 진행할거니까!!

```yaml
version: "3.7"

# 컴퓨터들
services:
  # 컴퓨터이름
  my-backend:
    image: asia.gcr.io/프로젝트ID/만들고싶은폴더이름:0.0
    platform: linux/x86_64
    build:
      context: .
      dockerfile: Dockerfile
    # volumes:
    #   - ./src:/myfolder/src
    # ports:
    # env_file:
    #   - ./.env.prod

  # 컴퓨터이름
  # my-database:
  #   image: mysql:latest
  #   environment:
  #     MYSQL_DATABASE: 'myproject'
  #     MYSQL_ROOT_PASSWORD: 'root'
  #   ports:
  #     - 3306:3306
```

`image: asia.gcr.io/프로젝트ID/만들고싶은폴더이름:0.0` : 컨테이너 레지스트리에서 이미지를 지정한다

`0.0`은 Push를 할때마다 버전을 올려야한다. 숫자가 바뀌어야 무언가 바뀌었다는걸 gcp가 감지한다.

숫자 안올리면 업데이트 안됨....!

<br>

<br>

주석처리한

```yaml
# 컴퓨터이름
# my-database:
#   image: mysql:latest
#   environment:
#     MYSQL_DATABASE: 'myproject'
#     MYSQL_ROOT_PASSWORD: 'root'
#   ports:
#     - 3306:3306
```

이 부분은 로컬에서 돌리는게 아니기때문에 주석처리해줌

<br>

<br>

`ports`, `env_file` 전부 쿠버네티스에 따로 설정해주는 부분이 있기때문에 적어줄 필요가 없기 때문에 주석처리

```yaml
my-backend:
  image: asia.gcr.io/프로젝트ID/만들고싶은폴더이름:0.0
  platform: linux/x86_64
  build:
    context: .
    dockerfile: Dockerfile
```

이 부분을 push하는데, 명령어는

`docker-compose -f docker-compose.prod.yaml build` 해주고

`docker-compose -f docker-compose.prod.yaml push` 이다.

<br>

🌟 push가 완료되는 순간, `asia.gcr.io/프로젝트ID/만들고싶은폴더이름:0.0` 이 경로를 찾아 빌드된 소스코드를 업로드한다.🌟

확인하는 방법은

![쿠버네티스](/assets/img/kubernetes/9.png)

`Container Registry` 의 이미지에 들어가면 이렇게 푸시된 이미지들을 볼 수 있다

(푸시 너무 여러번 한거 티나나... 푸시는 신중하게...ㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎ)

<br>

<br>

<br>

<br>

### Container Registry에 push된 코드들 컨테이너와 연결하기(작업 부하)

---

![쿠버네티스](/assets/img/kubernetes/5.png)

`클러스터` 를 다 만들었다면

이제는 `부하분산`에 들어가서 작업하기를 원하는 클러스터에 위치하는지 확인한다

`배포` 를 눌러 컨테이너를 만들어준다.

(나는 nest.js를 배포하려고 하므로 이름은 `mybackend-nestjs`로 할거임)

VM인스턴스로 배포를 할때는 코드를 git에 올려서 git pull로 우분투에서 가져온다음에 배포를 했는데,

쿠버네티스는 깃을 사용하지 않는다.

( 쿠버네티스에서는 Google Cloud SDK를 이용하는데,

이 방법은 나중에 다시 블로그로 작성해야지..! 이것도 상당히 중요한 내용이지만 여기서 얘기하면 너무 길어짐..! )

`docker-compose -f docker-compose.prod.yaml push` 를 제대로 하면

![쿠버네티스](/assets/img/kubernetes/6.png)

`container registry` 에 올라온 컨테이너 이미지를 선택할 수 있게 된다.

가장 최근에 push한 이미지를 선택하고 환경변수를 설정해준다.

VM인스턴스로 배포했을때는 git에 .env파일이 올라가지 않기때문에

`vi .env.docker`, `vi .env.prod` 파일들을 만들어줬었는데 쿠버네티스를 이용하면

![쿠버네티스](/assets/img/kubernetes/7.png)

이렇게 따로 환경변수를 지정할 수 있다.

환경변수를 지정안해주면 읽어올 .env파일이 없기떄문에 백퍼 에러난다~

이렇게 다 설정해놓고 밑에 구성 YAML을 보면 굉장히 복잡해 보이는 야믈파일이 생성이 되는데

![쿠버네티스](/assets/img/kubernetes/8.png)

이게 aws에서는 자동 생성이 되지 않는다고 하니... gcp에서 만들어서 yaml파일을 복사 하는것도 일종의 🍯팁이라고 한다!

<br>

<br>

<br>

<br>

### 브라우저에서 배포한 nestjs에 접속할 수 있게 하기

---

클러스터에 외부IP를 통해 노출을 시켜줘야 지금 올린 nestjs코드로 만들어진 어플리케이션에 접속할 수 있다.

이 노출은 `작업부하` 에서 해준다. 작업부하에서 `노출`을 해주면되는데,

확인은 `서비스 및 수신` 에서 할 수 있다.

![쿠버네티스](/assets/img/kubernetes/10.png)

<br>

<br>

배포된 작업부하에 들어가면 `노출 중인 서비스` 의 `노출` 버튼을 클릭한다.

![쿠버네티스](/assets/img/kubernetes/11.png)

여기서 서비스 유형을 선택해주어야 하는데,

db처럼 외부에 공개하는게 위험하고, 클러스터 내부에서만 접속가능하게 해주려면 `클러스터 ip` 를,

외부에서도 접속 가능하게 하려면 `부하 분산기` 를 선택하면 된다.

지금같은 경우는 외부에서 내 nestjs 프로젝트에 접근이 가능하도록 해주어야하므로 `부하분산기` 를 선택한다.

`포트`는 `3000`으로(nest가 3000으로 실행중이므로), `대상포트1`도 `3000`으로.

노출을 시켜주면 `서비스 및 수신`에서 확인을 해줄 수 있는데

![쿠버네티스](/assets/img/kubernetes/12.png)

보면 엔드포인트가 생성된것을 확인할 수 있다.

이 엔드포인트로 브라우저에서 접속하면 접속이 된다! (뒤에 그래프큐엘 붙여야함)

![쿠버네티스](/assets/img/kubernetes/13.png)

<br>

<br>

<br>

<br>

<br>

### 코드 수정

---

`package.json` 에 있는 `dependencies`(실행시 필요) 와 `devDependencies`(개발시 필요) 가 있는데

우리는 어짜피 이제 배포를 하게 되면 실행할때 필요한 디펜던시들만 있으면 되니까 굳이 데브디펜던시는 필요가 없다.

이때 사용하는 명령어가

`yarn start:prod` : 시작명령을 바꾸기. 이 명령어로는 `dist` 폴더안의 `main.js` 를 실행한다. (javascript실행)

`yarn install --production` : 데브디펜던시 말고 디펜던시만 설치함!

이렇게 하려면 도커파일을 바꿔야한다.

그래서 `Dockerfile.prod` 라는 파일을 새로 하나 만들어서

```dockerfile
FROM node:14

COPY ./package.json /myfolder/
COPY ./yarn.lock /myfolder/
WORKDIR /myfolder/
RUN yarn install --production

COPY . /myfolder/

RUN yarn build
CMD yarn start:prod
```

`RUN yarn build` 해줘야 dist폴더가 생성되니까.. 그래야지 main.js에 접근가능하니까..

이렇게 바꾼 `Dockerfile.prod` 를 `docker-compose.prod.yaml` 파일에 적용한다.

```yaml
#docker-compose.prod.yaml

version: "3.7"

services:
  my-backend:
    image: asia.gcr.io/프로젝트ID/만들고싶은폴더이름:버전
    platform: linux/x86_64
    build:
      context: .
      dockerfile: Dockerfile.prod
```

이제 99% 끝남.

근데 package.json을 보면 "build" 키의 값이 "nest build" 인걸 볼 수 있는데

이 `nest` 명령어를 사용하려면 `nestjs/cli` 가 있어야함.

근데 이 부분은 `devDependencies`에 있음

그래서 `"@nestjs/schematics": "^9.0.0"` 이 부분만 `dependencies` 로 옮겨준다

> `package.json` 의 `script` 부분을 보면
>
> ```
> "scripts": {
>     "prebuild": "rimraf dist",
>     "build": "nest build",
> ```
>
> `prebuild` 와 `build` 부분이 있는데, 세트로 다닌다.
>
> `prebuild`는 build전에 실행되는 명령어 인데, `prebuild` 의 값인 `"rimraf dist"` 이 부분은
>
> dist폴더를 삭제해 달라는 말임.
>
> 그리고 새로운 dist폴더를 빌드해 달라는 명령어.

 <br>

이제 이렇게 vscode를 수정했으니

`docker-compose -f docker-compose.prod.yaml build`

`docker-compose -f docker-compose.prod.yaml push` 로 다시 push해준다(prod.yaml파일의 버전 올려줘야한다!!!)

<br>

<br>

<br>

<br>

6시간이 넘는 강의 캡쳐해가면서 올리려니까 죽을맛이네....

원래는 하나로 끝내려 했는데 도저히 불가능이다.

2편으로 나눠서 써야지

2편에 계속.......투비컨티뉴....

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

<br>

<br>
