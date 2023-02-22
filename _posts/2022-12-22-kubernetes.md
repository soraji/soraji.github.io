---
layout: post
title: "[ K8s ] 쿠버네티스로 배포하기 2편 ( gcloud mysql과 연동, ssl인증서 생성 및 적용)"
categories: back
comments: true
---

<br>

<br>

# 쿠버네티스로 배포하기 2편

1편을 https://soraji.github.io/back/2022/12/21/kubernetes/ 을 읽고 오십쇼

<br>

<br>

<br>

<br>

### cloudSQL 데이터베이스 만들기(mysql)

---

구글 클라우드 SQL을 사용해서 nestjs와 연결해줘야하는데, DB가 없으니 db를 먼저 만들어줘야한다.

터미널에서 mysql을 설치하는게 아니라 gcloud SQL을 이용해서 연결하기로했으니까

SQL에 들어가서 만들자.

SQL에 들어가서 인스턴스 만들기 버튼을 클릭한다.

![쿠버네티스](/assets/img/kubernetes/14.png)

새로 만드는거, 삭제하는거 죄다 시간이 꽤나 걸린다. (Ծ‸ Ծ)

![쿠버네티스](/assets/img/kubernetes/15.png)

mysql로 디비를 만든다.

`인스턴스ID` 를 작성한다.

`비밀번호`는 `root`

`시작할 구성선택`에서는 `Development` (만약 `Production`을 선택하면 컴퓨터가 2대이기때문에 요금이 2배.....)

`리전`은 `asia-northeast3`

`영역가용성` 은 `단일영역`

비용을 줄이려면 `머신유형` 에서 cpu를 줄이고.. (별로 차이는 안난다.)

`연결` 에서는 `비공개IP` 를 선택한다. DB니까 공개되면 안됨. `네트워크` 는 `default` 를 선택.

그리고 `인스턴스 만들기` 클릭 => DB생성 완료! (생성되는데 15분정도 걸린다)

<br>

<br>

<br>

<br>

<br>

### gcloudSQL 연결

---

자 이제는 내가 push했던 nest.js 프로젝트에 방금 만든 gcloud SQL을 연결해보자.

![쿠버네티스](/assets/img/kubernetes/16.png)

이름은 쿠버네티스 `작업부하` 에서 배포했던 환경변수에 입력한 데이터베이스 이름으로 변경하면 된다.

만약 기억이 안난다면 (미래의 나 보고 있니?)

![쿠버네티스](/assets/img/kubernetes/17.png)

쿠버네티스의 `보안 비밀 및 ConfigMap` 에 들어가서 배포했던 컨테이너 이름으로 되어있는 config에 들어가보면

데이터 부분에 적어둔 데이터베이스 이름을 적어주면 된다.

(만약 환경변수를 적어줘야할 일이 있다면 이곳에서 수정하면 된다.)

이제 HOST부분을 연결해야하는데,

HOST부분은 SQL에서 확인하면 된다.

![쿠버네티스](/assets/img/kubernetes/18.png)

`비공개IP 주소` 를 복사해서 `보안 비밀 및 ConfigMap` 의 환경변수를 수정해서 연결시킨다.

![쿠버네티스](/assets/img/kubernetes/17.png)

이런식으로..

<br>

<br>

### 이미 실행중인 클러스터에 새롭게 push된 코드를 반영하도록 update해주기

---

근데 이제 바로 연결이 안되고 한번 재시작(혹은 업데이트)을 해주어야함.

물론 새로 만들어줄수도 있지만 클러스터도 그렇고 서비스도 그렇고 이것저것 새로 만들어주는건 너무 귀찮으니까..

이미 배포되어있는것에서 업데이트해주는게 사실 어찌보면 제일 간단한 방법.

<br>

<br>

재시작 혹은 업데이트는 터미널에서 할 수 있는데, 터미널에 들어가려면 클러스터에서 들어갈수있다

![쿠버네티스](/assets/img/kubernetes/19.png)

클러스터에서 현재 실행중인 클러스터에서 연결을 클릭한다

![쿠버네티스](/assets/img/kubernetes/20.png)

그러면 창이 뜨는데, 거기서 `CLOUD SHELL에서 실행` 버튼을 클릭한다.

클릭하게되면 브라우저 아래에 터미널창이 뜨는데, 거기서 자동으로 현재 클러스터의 위치를 잡는다.

![쿠버네티스](/assets/img/kubernetes/21.png)

자동으로 경로를 써주고 커서가 깜박이는데 그때 엔터 쳐주면 그 클러스터 터미널에 접속한거라고 보면 된다.

<br>

<br>

그 상태에서 이제는 cli명령어를 써준다.

업데이트를 하는 명령어를 써주어야 하는데, 업데이트를 해주려면 클러스터와 프로젝트와 등등 여러가지를 적어주어야 한다.

이때, 야믈파일에서 확인하는 방법이 있고, gui에서 확인하는 방법이 있는데,

gui에서 확인하는 방법은 `작업부하` 에서 배포한 컨테이너에 들어가면 `pods사양` 이렇게 적혀있다

![쿠버네티스](/assets/img/kubernetes/22.png)

이렇게 갖고오거나 아니면 yaml파일에서 가져오는 방법이 있는데,

명령어는 `kubectl get pods -o yaml` 이다.

이렇게 쳐주면 아래 그림처럼 확인할 수 있다.

![쿠버네티스](/assets/img/kubernetes/23.png)

<br>

<br>

이렇게 확인을 했으면 이제는 진짜 업데이트를 해보자 (제발하자 좀...( ༎ຶŎ༎ຶ ))

명령어는

```
$ kubectl set image deployment/작업부하배포이름 야믈파일에서추출한pods사양=containerRegistry에등록한이미지이름
```

이렇게이다.

예를들면 실제로는

`kubectl set image deployment/mybackend-nestjs new-backend-sha256-1=asia.gcr.io/new-backend-372105/new-backend:0.9` 이런식으로(나는 push를 9번해서 0.9버전...)

이런식으로 써줄수있겠다.

이렇게 업데이트를 해주면 터미널에

`deployment.apps/mybackend-nestjs image updated` 라고 뜰거다.

그리고나서 제대로 업데이트가 되었는지 확인하려면

```
$ kubectl get pods
```

이 명령어를 쳐주면 되고,

명령어를 써줌과 동시에

![쿠버네티스](/assets/img/kubernetes/24.png)

이렇게 pending이라고 뜬다. 새로 만들고있는중인거다.

새로만들어진 pods는 pending이었다가, ContainerCreating이었다가 시간이 조금 더 지나면 running으로 바뀐다.

그러면 기존에 running이었던 것들이 terminating으로 바뀌면서 사라진다.

<br>

<br>

자 그럼 이제 새로 생성된(=업데이트된) pods가 잘 돌고있는지 확인해보자.

잘 돌고 있는지 확인하려면

```
$ kubectl logs -f pods의NAME
```

을 써주면 되는데

예를 들어

![쿠버네티스](/assets/img/kubernetes/25.png)

빨간사각형 안의 NAME을 `-f` 뒤에 적어주면 된다. (너무 길고 랜덤str이라 안적어야지..)

그러면 로그들이 쫙 뜨면서 현재 상황을 알려준다.

문제가 없으면 접속과 sql이 잘 연결된거를 확인할 수 있다. (하 드뎌 끝이 보인다.....ㅠㅜㅠㅜㅠㅜㅠ)

<br>

<br>

추가적으로 알아두면 좋은 쿠버네티스 명령어

배포된 컨테이너 가져오기

```
$ kubectl get deployments
```

![쿠버네티스](/assets/img/kubernetes/26.png)

<br>

<br>

서비스 가져오기

```
$ kubectl get services
```

![쿠버네티스](/assets/img/kubernetes/27.png)

<br>

<br>

<br>

<br>

<br>

### SSL 인증서 생성하기 (ingress)

---

여태까지 클러스터에는 여러개의 노드가 있고 ( 보통3개 ), 노드안에는 pods가 존재한다는걸 배웠다

그리고 이 클러스터에 외부IP를 통해서 (외부ip설정 = 외부부하분산) 외부로의 접속이 가능하게 했고,

내부ip를 설정함으로서 클러스터 안에있는 노드들끼리는 통신이 가능하도록 했다.

클러스터에 접속할 수 있도록 외부ip를 설정하는것은 `서비스 및 수신`에서 `노출` 을 통해 가능하게 했다.

잘 모르겠으면 1편의 [브라우저에서 배포한 nestjs에 접속할 수 있게 하기](https://soraji.github.io/back/2022/12/21/kubernetes/) 를 보면 될거임..

![쿠버네티스](/assets/img/kubernetes/28.png)

<br>

<br>

근데 이제 저거는 `http` 로만 접속이 가능한데, ssl인증서를 설치해주면서 `https`의 접속을 가능하게 만들어줄거다.

(인증서가 발급되는것도 짧을수도 있지만 꽤 오래걸리므로 미리 해놓는게 좋음. 최대 24시간 걸리는걸로 알고있다)

이런 기능을 만들어주기 위해서는 `인그레스` 를 설정해주어야 한다.

> `인그레스` 란?
>
> 클러스터 안으로 들어오는것을 인그레스(`ingress`)
>
> 밖으로 나가는것을 이그레스(`egress`)라고 한다

인그레스는 `서비스 및 수신` 에서 설정할 수 있다.

배포된 서비스를 클릭하면 `인그레스 만들기` 버튼이 활성화가 된다.

![쿠버네티스](/assets/img/kubernetes/29.png)

선택을 안하면 활성화가 안됨!

<br>

<br>

`인그레스 만들기` 를 클릭하면 `Kubernetes 수신 만들기` 로 들어가지는데

![쿠버네티스](/assets/img/kubernetes/30.png)

로드밸런스와 굉장히 비슷하게 생겼다.

이름을 작성하고(나는 mybackend-ingress라고 했음)

`Backend configuration` 에서는 따로 해줄건 없고,

![쿠버네티스](/assets/img/kubernetes/31.png)

`Frontend configuration` 에서

`프로토콜` 을 `https`로,

`인증서`를 선택하는데 만들어놓은 인증서가 없기 때문에 `새 인증서 만들기` 를 클릭한다.

(나는 이미 만들어놓은게 있어서 가려놓았음)

<br>

<br>

<br>

<br>

![쿠버네티스](/assets/img/kubernetes/32.png)

`새 인증서 만들기` 에서 이름을 적어주고

`Create Google-magaged certificate` 을 선택한다.

그리고 도메일은 본인이 구매한 도메인 주소를 작성한다.

(도메인주소에 www를 넣으면 안됨. 본인이 구매한 도메인 이름만 넣을것..!

예시에 있어서 www를 넣고 만들었는데 인증서발급 3일동안 안되서 삭제한다음에 다시 만들었더니 그제서야 됨... 🫠)

그리고 `만들기` 버튼 클릭.

그러면 인증서에 방금 만든 인증서가 들어오게 되고, 그 인증서를 선택하면 된다.

그러면 `Backend configuration` 은 끝

<br>

따로 저장버튼을 누르지않아도 저장된다.

<br>

<br>

이제는 `Host and path rules` 을 설정해주어야 한다.

`호스트 및 경로 규칙 구성` 에서 어떤 부하분산과 연결시켜줄건지 물어보는 항목이다.

내 nest.js 프로젝트에 ssl을 입히는거이기때문에

노출되어있는(=외부ip가 있는) nest.js 서비스를 선택한다.

![쿠버네티스](/assets/img/kubernetes/33.png)

<br>

<br>

이제 `create` 로 인그레스를 만들어준다.

이게 생성되는데만 시간이 좀 걸리므로 기다려야한다.....!

<br>

<br>

<br>

<br>

TMI )

회사다녔을때 ssl적용 내가 했는데 나는 물리서버를 가지고 있는 회사였어서 ssl인증서를 구매하는 방법밖에 없었다.

그때 무료로 하는 방법 엄청 찾아봤었는데..

클라우드는 무료로 하는게 가능하구나.. 하나 또 배웠고, 신기했던 날.

<br>

<br>

<br>

### All backend services are in UNHEALTHY state 에러 해결

---

인그레스를 확인하려면 `서비스 및 수신` 안에 숨어있다....!

![쿠버네티스](/assets/img/kubernetes/36.png)

근데 문제는, `All backend services are in UNHEALTHY state` 라는 에러가 뜬다는거다.

건강하지 않은 상태는 또 무엇인가...... 모르는 사람은 진짜 멘붕올수밖에 없는 구조...

![쿠버네티스](/assets/img/kubernetes/37.png)

에러가 나는 이유를 확인해보면(확인 솔직히 안해도됨. 그냥 그렇구나 보면 됨)

`All backend services are in UNHEALTHY state` 에러가 나는 이유는

보면 경로가 `'/'` 로 되어있다.

`HealthChecker` 가 요청을 했을때 response가 200을 받아와야 '아 얘가 살아있구나'를 로드밸런스에 알려주는데

우리가 배포한 nest.js프로젝트의 엔드포인트에 `/` 가 없으니 받아오는 값이 없어서 죽었다고 생각하는것이다.

그러니 결국에는 우리는 vscode로가서(=nest.js프로젝트) 코드에 엔드포인트가 `/` 인 api를 하나 만들어주어야 한다.

`controller` 가 restAPI를 만드는 곳이므로 `app.module.ts` 의 형제 파일로 `app.controller.ts` 를 하나 만들어준다.

```ts
//app.controller.ts

import { Controller, Get } from "@nestjs/common";

@Controller()
export class AppController {
  @Get("/")
  getHello(): string {
    return "상태확인완료";
  }
}
```

이렇게 엔드포인트가 `/`인 restAPI를 만들어주고

`app.module.ts` 에 추가해준다.

```ts
//app.module.ts

import { ApolloDriver, ApolloDriverConfig } from "@nestjs/apollo";
import { CacheModule, Module } from "@nestjs/common";
import { GraphQLModule } from "@nestjs/graphql";
import { TypeOrmModule } from "@nestjs/typeorm";
import { ConfigModule } from "@nestjs/config";
import { AppController } from "./app.controller";

@Module({
  imports: [...중략],
  providers: [...중략],
  controllers: [AppController], //추가
})
export class AppModule {}
```

로컬에서 돌려보면 이런식으로 맵핑된걸 볼 수 있다.

![쿠버네티스](/assets/img/kubernetes/38.png)

이러면 잘 연결된것.

<br>

<br>

<br>

자.. 여기서 끝이 아니에요... ( ˙ỏ˙ )

[1편에서 했던](https://soraji.github.io/back/2022/12/21/kubernetes/) 코드 push하고 (docker-compose.prod.yaml 버전 올리고 push)

2편의 **이미 실행중인 클러스터에 새롭게 push된 코드를 반영하도록 update해주기** 에서 했던 명령어로

```
$ kubectl set image deployment/작업부하배포이름 야믈파일에서추출한pods사양=containerRegistry에등록한이미지이름
```

로 다시 업데이트를 해줘야지 지금 방금 잘 mapping된게 반영이 된다.

<br>

코드상 변경이 되는 부분이 있다면 매번 이런식으로 반영(push) => 업데이트를 해줘야한다.

그리고 `kubectl get pods` 로 잘 돌아가고 있는지 확인해주고, `kubectl logs ...` 로 잘 돌아가는지, 잘 올라갔는지 확인해주기.

<br>

에러처리하는데도 시간이 꽤 걸린다.

좀 기다려줘야함.

<br>

<br>

<br>

### DNS(도메인)에 만들어놓은 ssl 인증서 입혀주기

---

인그레스에서 `프런트엔드` 의 ip를 복사한뒤에

`네트워크 서비스 > Cloud DNS` 로 들어간다.

(DNS를 만드는법은 ... vm인스턴스할때 했네. 이것도 나중에 포스팅해야겠다)

만들어진 DNS에 들어가서 수정버튼 누르고

![쿠버네티스](/assets/img/kubernetes/39.png)

<br>

<br>

`IPv4 주소` 를 아까 인그레스에서 만들어놓은 `프론트엔드` ip로 변경해준다.

![쿠버네티스](/assets/img/kubernetes/40.png)

<br>

<br>

인그레스에서 만들어놓은 `프론트엔드` ip는 `서비스 및 수신 > 인그레스` 에서 확인할 수 있다.

![쿠버네티스](/assets/img/kubernetes/35.png)

<br>

<br>

<br>

<br>

이제 어느정도 시간 지나면....!!!!!!!

Https 로 시작되는 도메인 뒤에 graphql 붙이면 연결된다!!!!!

![쿠버네티스](/assets/img/kubernetes/41.png)

다른 사람 컴퓨터에서 접속해도 접속됨!
