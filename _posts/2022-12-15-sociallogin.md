---
layout: post
title: "[ NestJS ] 구글, 카카오, 네이버 소셜로그인"
categories: back
comments: true
---

<br>

<br>

<br>

<br>

# 소셜로그인 with google, kakao, naver

요즘 어딜가든 보이는 소셜로그인... 예전부터 엄청 구현해보고 싶었는데

몇주전에 구현해보고 정리한다 정리한다 그랬는데.. 너무 길어서.. 드디어 정리한다...!

우선 로그인프로세스에 대해서 알아야하는데, ~~예전에 쓴 글이 로그인하기에는 정보가 턱없이 부족하네...? 다시 수정해야겠다..~~

자체 로그인서비스와는 다르게 소셜로그인을 통해서 회원가입 & 로그인을 시켜준다는데 있다

![소셜로그인](/assets/img/2022-12-15/sociallogin.png)

위 그림에서 보듯이 유저가 소셜미디어를 통해 로그인을 하려고 한다.

이때 뒤에서 일어나는 순서( 아래 1~8번 )들이 ⭐️상당히 중요⭐️한데,

1. 구글로그인 버튼을 눌러 페이지를 요청한다.
2. 백엔드 api에서는 소셜로그인을 시도하는 유저에게 소셜미디어의 HTML코드를 넘겨준다
3. 유저는 받은 HTML에서 본인이 로그인하고 싶은 사용자를 선택하고, 그러면 소셜미디어로 로그인을 하게됨
4. 소셜미디어에서 올바르게 로그인을 하게되면 브라우저로 시크릿코드를 보내준다
5. 브라우저에서는 시크릿코드를 백엔드서버로 보내준다
6. 백엔드서버는 이 시크릿코드를 소셜미디어에 다시 보내면서 '니가 보낸 시크릿코드가 이게 맞나?' 하면서 물어본다
7. 소셜미디어에서 확인해보고 일치하면 accessToken과함께 유저 프로필을 보내준다.
8. 백엔드서버에서는 받아온 프로필로 정보가 없으면 회원가입으로 보내고, 있으면 로그인을 완료시킨다

⭐️화살표 방향을 보면서 순서 & 큰그림을 보는게 매우매우 중요하다⭐️

<br>

<br>

---

## 구글 Google

그러면, 실습을 해보자

Nest.js에서 `src`폴더안에 `auth`폴더를 만들고,

`auth.controller.ts` 파일안에 이렇게 코드를 짠다.

```ts
// auth.controller.ts

import { Controller, Get, Req, Res, UseGuards } from "@nestjs/common";
import { AuthGuard } from "@nestjs/passport";
import { Request, Response } from "express";
import { UsersService } from "../users/users.service";
import { AuthService } from "./auth.service";

interface IOAuthUser {	//interface 설정
  user: {
    name: string;
    email: string;
    password: string;
  };
}

@Controller()	//컨트롤러 데코레이터
export class AuthController {	//클래스이름
  constructor(	//컨스트럭터로 초기값설정
    private readonly usersService: UsersService, //DI 해주기
    private readonly authService: AuthService
  ) {}

  //-----------------------구글 로그인-----------------------------//
  @Get("/login/google")	//restAPI만들기. 엔드포인트는 /login/google.
  @UseGuards(AuthGuard("google"))	//인증과정을 거쳐야하기때문에 UseGuards를 써주고 passport인증으로 AuthGuard를 써준다. 이름은 google로
	async loginGoogle(
    @Req() req: Request & IOAuthUser,
    @Res() res: Response	//Nest.js가 express를 기반으로 하기때문에 Request는 express에서 import한다.
  ) {
     //프로필을 받아온 다음, 로그인 처리해야하는 곳(auth.service.ts에서 선언해준다)
    this.authService.OAuthLogin({req, res});

}

```

`@UseGuards(AuthGuard("google"))` 이렇게 인증과정을 거쳐야하는 부분은

`src/commons/auth/jwt-social-google.strategy.ts` 라는 파일을 하나 만들고

이 passport인증과정을 거치는 ts파일을 하나 만들어준다

그리고 passport를 사용하기 위해

`yarn add passport-google-oauth20` 으로 설치해준다.

ts도 설치해줘야하기때문에 `yarn add --dev @types/passport-google-oauth20` 도 설치해주어야함

(카카오 네이버도 각자만의 passport lib가 있기때문에 각각 다 설치해줘야한다)

```ts
// jwt-social-google.strategy.ts

import { PassportStrategy } from "@nestjs/passport";
import { Strategy } from "passport-google-oauth20";

export class JwtGoogleStrategy extends PassportStrategy(Strategy, "google") {
  //UseGuards의 이름과 동일해야함
  constructor() {
    //constructor에서 성공하면 아래의 validate로 넘겨주고, 만약 실패하면 멈춰지고 에러 반환
    super({
      //자식의 constructor를 부모의 constructor에 넘기는 방법은 super를 사용하면 된다.
      clientID: process.env.GOOGLE_CLIENT_ID, //.env파일에 들어있음
      clientSecret: process.env.GOOGLE_CLIENT_SECRET, //.env파일에 들어있음
      callbackURL: process.env.GOOGLE_CALLBACK_URL, //.env파일에 들어있음
      scope: ["email", "profile"],
    });
  }

  validate(accessToken, refreshToken, profile) {
    // console.log(accessToken);
    // console.log(refreshToken);
    // console.log(profile);

    return {
      name: profile.displayName,
      email: profile.emails[0].value,
      hashedPassword: "1234",
    };
  }
}
```

- `constructor` : 인증하는 부분
- `validate` : 인증결과를 받는 부분

`auth.controller.ts`에서 보면 `this.authService.OAuthLogin({req, res});` 부분이 있는데

`auth.service.ts` 파일에서 `OAuthLogin` 함수를 실행하라는 뜻이므로

```ts
import { Injectable } from "@nestjs/common";
import { JwtService } from "@nestjs/jwt";
import { UsersService } from "../users/users.service";
import {
  IAuthServiceGetAccessToken,
  IAuthServiceSetRefreshToken,
} from "./interfaces/auth-service.interface";

@Injectable()
export class AuthService {
  constructor(private readonly usersService: UsersService) {}

  async OAuthLogin({ req, res }) {
    // 1. 회원조회
    let user = await this.usersService.findOne({ email: req.user.email }); //user를 찾아서

    // 2, 회원가입이 안되어있다면? 자동회원가입
    if (!user) user = await this.usersService.create({ ...req.user }); //user가 없으면 하나 만들고, 있으면 이 if문에 들어오지 않을거기때문에 이러나 저러나 user는 존재하는게 됨.

    // 3. 회원가입이 되어있다면? 로그인(AT, RT를 생성해서 브라우저에 전송)한다
    this.setRefreshToken({ user, res });
    res.redirect("리다이렉트할 url주소");
  }
}
```

이렇게 `controller`, `servce`에 등록을 해주었다면 `auth.module.ts`에도 등록해주어야한다.

```ts
// auth.module.ts

import { Module } from "@nestjs/common";
import { TypeOrmModule } from "@nestjs/typeorm";
import { User } from "../users/entities/user.entity";
import { UsersService } from "../users/users.service";
import { AuthResolver } from "./auth.resolver";
import { AuthService } from "./auth.service";
import { JwtModule } from "@nestjs/jwt";
import { JwtRefreshStrategy } from "src/commons/auth/jwt-refresh.strategy";
import { JwtAccessStrategy } from "src/commons/auth/jwt-access.strategy";
import { AuthController } from "./auth.controller";
import { JwtGoogleStrategy } from "src/commons/auth/jwt-social-google.strategy";
import { JwtNaverStrategy } from "src/commons/auth/jwt-social-naver.strategy";
import { JwtKakaoStrategy } from "src/commons/auth/jwt-social-kakao.strategy";

@Module({
  imports: [
    JwtModule.register({}),
    TypeOrmModule.forFeature([
      User, //
    ]),
  ],
  providers: [
    JwtAccessStrategy, //accessToken
    JwtRefreshStrategy, //refreshToken
    JwtGoogleStrategy, //google소셜로그인
    JwtNaverStrategy, //naver소셜로그인
    JwtKakaoStrategy, //kakao소셜로그인
    AuthResolver, //resolver 주입
    AuthService, //service 주입
    UsersService, //user폴더의 service 주입
  ],
  controllers: [
    AuthController, //컨트롤러 주입
  ],
})
export class AuthModule {}
```

이렇게 등록을 해주면 docker로 띄울때 터미널에서 초록색으로

`Mapped {/login/google, GET} route` 라고 뜨면 연결이 잘 된거다.

우선은 AT는 받지않고 RF만 받기로 했으니까

브라우저에 개발자도구의 Application에 cookie안에 refresh Token이 생겼는지 확인하면 되고,

되었다면 잘 로그인된거라고 생각하면 된다.

> _소셜로그인에서 RefreshToken 만 받아오는 이유?_
>
> 원래는 자체 서비스에서 회원가입을 하게되면 accessToken(이하 AT )과 refreshToken(이하 RT)을 받아오는데
>
> AT는 req.body에 담겨오게 되고 RF는 cookie에 담겨오게 된다.
>
> 그런데 소셜로그인같은 경우에는 자체 html을 사용하지 않고 소셜미디어의 HTML을 사용하므로 받아올 body가 없다.
>
> 그래서 RF만 넘겨주면 프엔개발자들이 이 RF을 받아서 restoreToken API로 AT를 재발급받아서 사용할수 있다.
>
> 그래서 RF만 받아와도 괜찮음!

---

## KAKAO, NAVER 로그인

코드화 하기 전에 각 사이트의 개발자 사이트에서 설정해주는게 더 중요하다.

왜냐면 kakao와 naver에 '너희 기능좀 쓸게' 라고 허락을 구해야 소셜로그인 기능을 사용할 수 있기 때문이다.

코드를 아무리 잘써도 허가를 못받으면 아예 못씀...

카카오랑 네이버랑 조금 달라서 각각 설정해줘야하는게 조금 다른데,

네이버 먼저 설정하겠음. 왜냐면 조금 더 간단해서..?

<br>

<br>

<br>

### 네이버 로그인

우선은 [네이버개발자](https://developers.naver.com/main/) 사이트에 가서 내 어플리케이션 만들기를 진행한다.

알아서 본인상태에 맞게 만들기.

그리고 `Application > 어플리케이션 등록 ` 에 가서 신청을 하면 된다.

어플리케이션 이름을 적고,

사용API의 셀렉트박스에서 `네이버 로그인` 을 선택한다.

그러면 로그인했을때 받아올 수 있는 정보들을 체크하는 곳이 나오는데, 이걸로 어떤 정보를 받아올지 설정할 수 있다

![소셜로그인](/assets/img/2022-12-15/naver1.png)

어떤 api를 사용할건지 결정하면 그 아래로 callback url이나 서비스url을 입력하라고 하는데

본인이 원하는 url을 적어주면 된다.

서비스URL은 로그인 요청을 보내는 HTML주소라고 생각하면 되고,

콜백URL은 최종적으로 결과를 받는 내가 만든 api 주소라고 생각하면 된다.

![소셜로그인](/assets/img/2022-12-15/naver2.png)

그리고 `내 어플리케이션 > 어플리케이션 정보` 에서 `Client ID` 와 `Client Secret` 이 있는데 그걸 `.env` 파일에 넣어준다.

<br>

<br>

<br>

<br>

### 카카오 로그인

카카오에서도 동일하게 어플리케이션을 만들어준다.

`내 어플리케이션 > 앱 설정 > 요약정보` 에 들어오면 `앱 키` 가 있는데, 거기서 `REST-API` 가 우리가 쓸 clientID이다.

아래의 앱ID가 아님 ❌

![소셜로그인](/assets/img/2022-12-15/kakao1.png)

다음으로 `카카오 로그인` 에 들어가서 `활성화 설정` 에서 `OFF` 라면 `ON` 으로 바꿔주어야 한다.

처음에는 항상 OFF인것 같다. 나는 이미 활성화 상태임

그리고 RedirectURL이 콜백url이므로 api주소를 적으면 된다.

![소셜로그인](/assets/img/2022-12-15/kakao2.png)

카카오는 조금 다르게 secret키가 없어도 된다고 듣긴했는데,

혹시 모르니까 적어놓는다.

`카카오 로그인 > 보안` 에 가보면 `Client Secret` 이 있는데 거기서 `코드 생성` 을 누르면 secret 키가 발급된다.

![소셜로그인](/assets/img/2022-12-15/kakao3.png)

<br>

<br>

( 휴... 🫠 이렇게 정성스럽게 블로깅한거 진짜 오랜만이다....

사진 웬만하면 안넣는데.... )

<br>

<br>

<br>

---

코드로직은 구글과 똑같다.

```ts
// auth.controller.ts

import { Controller, Get, Req, Res, UseGuards } from "@nestjs/common";
import { AuthGuard } from "@nestjs/passport";
import { Request, Response } from "express";
import { UsersService } from "../users/users.service";
import { AuthService } from "./auth.service";

interface IOAuthUser {
  user: {
    name: string;
    email: string;
    password: string;
  };
}

@Controller()
export class AuthController {
  constructor(
    private readonly usersService: UsersService, //
    private readonly authService: AuthService
  ) {}

  //-----------------------구글 로그인-----------------------------//
  @Get("/login/google")
  @UseGuards(AuthGuard("google"))
  async loginGoogle(
    @Req() req: Request & IOAuthUser, //
    @Res() res: Response
  ) {
    this.authService.OAuthLogin({ req, res });
  }

  //-----------------------카카오 로그인-----------------------------//
  @Get("/login/kakao")
  @UseGuards(AuthGuard("kakao"))
  async loginKakao(
    @Req() req: Request & IOAuthUser, //
    @Res() res: Response
  ) {
    this.authService.OAuthLogin({ req, res });
  }

  //-----------------------네이버 로그인-----------------------------//
  @Get("/login/naver")
  @UseGuards(AuthGuard("naver"))
  async loginNaver(
    @Req() req: Request & IOAuthUser, //
    @Res() res: Response
  ) {
    this.authService.OAuthLogin({ req, res });
  }

  @Get("favicon.ico")
  favicon(
    @Req() req: Request & IOAuthUser, //
    @Res() res: Response
  ) {
    res.status(204).end();
  }
}
```

<br>

<br>

<br>

<br>

passport는 소셜미디어마다 다르기 때문에 다른 파일을 하나 만들어주어야한다.

`jwt-social-kakao.strategy.ts`

```ts
import { PassportStrategy } from "@nestjs/passport";
import { Strategy } from "passport-kakao";

export class JwtKakaoStrategy extends PassportStrategy(Strategy, "kakao") {
  constructor() {
    super({
      clientID: process.env.KAKAO_CLIENT_ID,
      clientSecret: process.env.KAKAO_CLIENT_SECRET,
      callbackURL: process.env.KAKAO_CALLBACK_URL,
      scope: ["account_email", "profile_nickname"],
    });
  }

  async validate(accessToken: string, refreshToken: string, profile: any) {
    // console.log('accessToken'+accessToken)
    // console.log('refreshToken'+refreshToken)
    // console.log(profile)
    // console.log(profile._json.kakao_account.email)

    return {
      name: profile.displayName,
      email: profile._json.kakao_account.email,
      password: profile.id,
    };
  }
}
```

<br>

<br>

<br>

`jwt-social-naver.strategy.ts`

```ts
import { PassportStrategy } from "@nestjs/passport";
import { Strategy } from "passport-naver";

export class JwtNaverStrategy extends PassportStrategy(Strategy, "naver") {
  constructor() {
    super({
      clientID: process.env.NAVER_CLIENT_ID,
      clientSecret: process.env.NAVER_CLIENT_SECRET,
      callbackURL: process.env.NAVER_CALLBACK_URL,
    });
  }

  validate(accessToken: string, refreshToken: string, profile: any) {
    // console.log(accessToken);
    // console.log(refreshToken);
    // console.log(profile);

    return {
      name: profile.displayName,
      email: profile._json.email,
      password: profile.id,
    };
  }
}
```
