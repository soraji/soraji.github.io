---
layout: post
title: "[ NestJS ] 결제 프로세스"
categories: back
comments: true
---

지금 정리해놔야하는게 소셜로그인 프로세스랑... 결제검증 프로세스인데...

너무 길어서 엄두가 안난다 징쨔... ᕙ( ︡’︡ 益’︠)ง

_2022.12.16 update [소셜로그인프로세스](https://soraji.github.io/back/2022/12/15/sociallogin/)는 글 썼다!_

<br>

<br>

결제검증 로직은, 코드를 막 갖다붙이는게 아니라 어디서 어떤 로직으로 갖다 붙일건지가 사실 더 핵심이고

이것보다 더 중요한게 docs를 해석할 줄 아는 능력이다.

처음보는 사람들은 분명 절절맬것이고(나도 그랬음)

복붙을 하라고 이렇게 친절하게 오픈북 마냥 알려주고있는데도

복붙조차 하지 못하는 나 자신을 보면서 '세상에 이런 똥멍청이가 있나' 현타가 오지게 올것이다... ༎ຶ‿༎ຶ

하지만 처음이라면 그거시 당연한것.......... 그걸 겪고 한번 해봐야 어느정도 독스로 커스터마이징을 하는 능력이 키워지게 된다.

rest api가 처음이라 못하겠다고 많이들 말하지만..

바보야 문제는 그게 아니야.. ˣ˷ˣ 독스를 읽고 내가 처해진 환경에 어떤식으로 적용할 수 있느냐가 문제지...

그것만 파악 가능하다면 정말 그냥 복붙이다.

각설하고.. 어떻게 짰고, 기능을 만들었는지 시작해보쟈

<br>

<br>

<br>

결제는 PG사를 이용해야하는데 (일일히 모든걸 다 만들다가는... 진짜 D지는 수가 있다....‧₊˚(✘﹏✘) )

나는 [아임포트](https://www.iamport.kr/) 를 이용했다. 가입하고 로그인해주기.

[아임포트 어드민 페이지](https://admin.iamport.kr/auth/signin) 로 들어간다.

그리고 [아임포트 독스](https://chai-iamport.gitbook.io/iamport/) 를 새탭으로 띄운다. 이 독스안에 모오오오오오오오든 내용이 다 들어있다.

진짜 친절한 오픈북이라고 생각하면 됨.

 <br>

<br>

우선 내가 만들고 싶은 기능은, 결제 & 결제 검증 & 취소 기능이다.

하나씩 해보도록 하자

딱딱 나눠서 하고싶었는데, 코드라는게 그렇게 순서상으로 쓸 수가 없는거라...

다들 얼기설기 섞여있다 ㅠㅠ

그래서 최종코드 올리고 옆에 주석을 쓰던지 잘라내서 쓰던지 해야겠다..

우선 만들어주어야할 폴더와 파일에는

`payment` 폴더에 `payment.module.ts`, `payment.resolver.ts`, `payment.services.ts`,

`iamport` 폴더에 `iamport.service.ts`

이렇게 4개의 파일에 모든 결제관련 함수들을 다 넣어주면 된다.

# 1. 결제

결제는 우선 프론트엔드에서 결제창을 띄워주는것까지 해놓아야 한다.

[아임포트 독스의 3. 결제요청하기](https://chai-iamport.gitbook.io/iamport/auth/guide/3.) 에 잘 나와있음 (codepen도 해놓으심... 진짜 친절하네)

그래서 독스에는 이렇게 갖고오라고 나와있는데, ⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- jQuery -->
    <script
      type="text/javascript"
      src="https://code.jquery.com/jquery-1.12.4.min.js"
    ></script>
    <!-- iamport.payment.js -->
    <script
      type="text/javascript"
      src="https://cdn.iamport.kr/js/iamport.payment-1.2.0.js"
    ></script>
    <script>
      var IMP = window.IMP;
      IMP.init("impXXXXXXXXX");

      function requestPay() {
        IMP.request_pay(
          {
            pg: "kcp.{상점ID}",
            pay_method: "card",
            merchant_uid: "57008833-33004",
            name: "당근 10kg",
            amount: 1004,
            buyer_email: "Iamport@chai.finance",
            buyer_name: "아임포트 기술지원팀",
            buyer_tel: "010-1234-5678",
            buyer_addr: "서울특별시 강남구 삼성동",
            buyer_postcode: "123-456",
          },
          function (rsp) {
            // callback
            if (rsp.success) {
              console.log(rsp);
            } else {
              console.log(rsp);
            }
          }
        );
      }
    </script>
    <meta charset="UTF-8" />
    <title>Sample Payment</title>
  </head>
  <body>
    <button onclick="requestPay()">결제하기</button>
    <!-- 결제하기 버튼 생성 -->
  </body>
</html>
```

이걸 실제로 적용할때는 ⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️이렇게 써준다.

우선 헤더에 필요한 라이브러리들은 cdn으로 가져오자. (axios, jquery, iamport 라이브러리를 가져왔음)

```html
//payment.html

<!DOCTYPE html>
<html lang="ko">
  <head>
    <title>결제페이지</title>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <!-- jQuery -->
    <script
      type="text/javascript"
      src="https://code.jquery.com/jquery-1.12.4.min.js"
    ></script>
    <!-- iamport.payment.js -->
    <script
      type="text/javascript"
      src="https://cdn.iamport.kr/js/iamport.payment-1.2.0.js"
    ></script>
    <script>
      const requestPay = () => {
        const IMP = window.IMP; // 생략 가능
        IMP.init("impxxxxxxxxx"); //본인의 가맹점 번호
        // IMP.request_pay(param, callback) 결제창 호출
        IMP.request_pay(
          {
            // param
            pg: "nice", //이거 본인이 사용하는 pg사 이름 안써주면 안뜸(나는 nice를 쓰느라고 nice로 바꿈)
            pay_method: "card",
            // merchant_uid: "ORD20180131-0000011", //주문번호 겹치면 에러남(주석하면 랜덤으로 생성됨). imp_uid나 merchant_uid 둘중에 하나만 있으면 되서 이거는 주석처리함
            name: "스타벅스 텀블러",
            amount: 100,
            buyer_email: "gildong@gmail.com",
            buyer_name: "홍길동",
            buyer_tel: "010-4242-4242",
            buyer_addr: "서울특별시 강남구 신사동",
            buyer_postcode: "01181",
          },
          function (rsp) {
            // callback
            if (rsp.success) {
              axios.post(
                "http://localhost:3000/graphql",
                {
                  query: `
                mutation{
                  createPayment(
                    impUid : "${rsp.imp_uid}", amount: ${rsp.paid_amount}
                  ){
                    id
                  }
                }
              `,
                },
                {
                  headers: {
                    Authorization:
                      "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InFAcS5jb20iLCJzdWIiOiI0Y2I1Zjg3ZS0wNWU2LTQwNjMtOTcxYy0xMzUzOTY4Njc4MDQiLCJpYXQiOjE2Njk5NzQwNjgsImV4cCI6MTY2OTk3NzY2OH0.DiKyobAKVxiJYTwMyB_DcXBF1QdSiHvTv1c30vFBI0o",
                  },
                }
              );

              alert("결제 성공 🥳");
              rsp.impUid;
              rsp.amount;
            } else {
              alert("결제 실패 🥺");
            }
          }
        );
      };
    </script>
  </head>
  <body>
    <button class="btnClick" onclick="requestPay()">결제하기</button>
  </body>
</html>
```

<br>

<br>

이부분이 graphql과 연결하는 부분인데,

```Js
axios.post('http://localhost:3000/graphql', {
  "query": `
    mutation{
      createPayment(
        impUid : "${rsp.imp_uid}", amount: ${rsp.paid_amount}
      ){
        	id
        }
      }
    `
  },{
  headers : {
  Authorization : "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InFAcS5jb20iLCJzdWIiOiI0Y2I1Zjg3ZS0wNWU2LTQwNjMtOTcxYy0xMzUzOTY4Njc4MDQiLCJpYXQiOjE2Njk5NzQwNjgsImV4cCI6MTY2OTk3NzY2OH0.DiKyobAKVxiJYTwMyB_DcXBF1QdSiHvTv1c30vFBI0o",
  }
})
```

graphql을 rest api로 쓸 수 있는 방법이다.

rest api를 쓰려면 axios를 써야하기 때문에 axios를 cdn으로 import 해왔다.

`Bearer` 뒤에는 로그인 한 사용자의 엑세스 토큰을 발급받아 넣어주어야 한다.

안그럼 인증 자체가 안되서 결제를 못함.

<br>

<br>

<br>

프엔에서는 저렇게 받고, 백엔드에서는

`payment.resolver.ts` 에서는

```ts
//payment.resolver.ts

import { UseGuards } from "@nestjs/common";
import { Args, Context, Int, Mutation, Resolver } from "@nestjs/graphql";
import { GqlAuthAccessGuard } from "src/commons/auth/gql-auth.guard";
import { IContext } from "src/commons/types/context";
import { IamPortService } from "../iamport/iamport.service";
import { Payment } from "./entities/payment.entity";
import { PaymentService } from "./payments.service";

@Resolver()
export class PaymentResolver {
  constructor(
    private readonly paymentService: PaymentService,

    private readonly iamPortService: IamPortService //새로운 iamport.ts를 만들어서 iamport관련 함수들은 다 모아두기
  ) {}

  @UseGuards(GqlAuthAccessGuard) //로그인한 유저만 가능하도록
  @Mutation(() => Payment) //graphql설정
  async createPayment(
    //결제 테이블에 데이터를 생성
    @Args("impUid") impUid: string,
    @Args({ name: "amount", type: () => Int }) amount: number, //그냥 number만 쓰게되면 1,000.0 이런식으로 소수점도 들어가기때문에 타입을 Int로 따로 또 한번 지정해주는게 필요하다. name은 amount로 쓰는대신 type은 Int로 하겠다는 타입지정이 한번 더 필요함.
    @Context() context: IContext
  ): Promise<Payment> {
    //아임포트 서버에 결제완료 기록이 존재하는지 확인
    const token = await this.iamPortService.fetchToken(impUid);
    const paymentData = await this.iamPortService.fetchPaymentData({
      impUid,
      token,
      amount,
    });

    //impuid가 존재하는지 중복결과 체크
    await this.iamPortService.checkDuplicate({ impUid });

    const user = context.req.user;
    return this.paymentService.create({ impUid, amount, user, paymentData });
  }
}
```

- `const token = await this.iamPortService.fetchToken( impUid );` : 아임포트 서버에 결제완료 기록이 존재하는지 확인을 해야하는데, 이 기록을 받아보려면 imp_uid가 무조건 있어야한다. 그래서 iamportservice 에서 token을 받아오는 `fetchToken` 함수로 토큰을 받아옴

<br>

<br>

<br>

<br>

`payment.service.ts` 에서는

```ts
//payment.service.ts

import { Injectable, UnprocessableEntityException } from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm";
import { DataSource, Repository } from "typeorm";
import { User } from "../users/entities/user.entity";
import {
  Payment,
  POINT_TRANSACTION_STATUS_ENUM,
} from "./entities/payment.entity";
import {
  IPaymentsServiceCreate,
  IPaymentsServiceCancel,
} from "./interface/payment-service.interface";

@Injectable()
export class PaymentService {
  constructor(
    @InjectRepository(Payment)
    private readonly paymentsRepository: Repository<Payment>,

    @InjectRepository(User)
    private readonly userRepository: Repository<User>,

    private readonly dataSource: DataSource
  ) {}

  //결제 테이블에 결제 내역 생성
  async create({
    impUid,
    amount: _amount,
    user: _user,
    status = POINT_TRANSACTION_STATUS_ENUM.PAYMENT,
    paymentData,
  }): Promise<Payment> {
    //status = POINT_TRANSACTION_STATUS_ENUM.PAYMENT은 디폴트값을 설정해주는 문법. 만약에 어디선가 status에 새로운 값이 있으면 새로운 값으로 넣어준다
    const payment = this.paymentsRepository.create({
      impUid,
      amount: _amount,
      user: _user,
      status,
    });

    //결제 데이터를 결제 테이블에 추가
    await this.paymentsRepository.save(payment);

    //user테이블에 동기화(누적금액으로)
    const user = await this.userRepository.findOne({ where: { id: _user.id } });
    console.log(
      `유저가 이미 결제한 내역은 ${user.paid}이며, 이번에 새로 결제한 금액은 ${_amount}입니다`
    );

    await this.userRepository.update(
      { id: user.id },
      { paid: user.paid + _amount }
    );

    //결제 데이터를 클라이언트에 리턴
    return payment;
  }
}
```

<br>

<br>

<br>

<br>

`iamport` 라는 폴더를 만들어서 `iamport.service.ts` 파일을 만들고 그 안에 iamport와 관련된 비즈니스로직들을 작성한다.

```ts
//iamport.service.ts

import {
  ConflictException,
  HttpException,
  Injectable,
  UnprocessableEntityException,
} from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm";
import axios from "axios";
import { Repository } from "typeorm";
import { resourceLimits } from "worker_threads";
import { Payment } from "../payments/entities/payment.entity";
import { IPaymentsServiceFetchData } from "../payments/interface/payment-service.interface";
import { User } from "../users/entities/user.entity";

@Injectable()
export class IamPortService {
  constructor(
    @InjectRepository(Payment)
    private readonly paymentsRepository: Repository<Payment>
  ) {}

  async checkDuplicate({ impUid }) {
    const result = await this.paymentsRepository.findOne({
      where: { impUid: impUid },
    });
    if (result) throw new ConflictException("이미 결제가 완료되었습니다");
  }

  //결제정보 조회하기
  async fetchToken(impUid: string): Promise<string> {
    try {
      //아임포트에서 토큰을 받아오기
      const getToken = await axios({
        url: "https://api.iamport.kr/users/getToken",
        method: "post", // POST method
        headers: { "Content-Type": "application/json" },
        data: {
          imp_key: process.env.IAMPORT_KEY,
          imp_secret: process.env.IAMPORT_SECRET,
        },
      });

      const access_token = await getToken.data.response.access_token; // 인증 토큰
      return access_token;
    } catch (e) {
      console.log(e);
      throw new HttpException(e.response.data.message, e.response.status);
    }
  }

  //결제정보 아임포트에서 가져오기
  async fetchPaymentData({
    impUid: imp_uid,
    token,
    amount,
  }: IPaymentsServiceFetchData) {
    try {
      const getPaymentData = await axios({
        url: `https://api.iamport.kr/payments/${imp_uid}`,
        method: "get",
        headers: {
          "Content-Type": "application/json",
          Authorization: token,
        },
      });

      if (getPaymentData.data.response.status !== "paid")
        throw new ConflictException("결제 내역이 존재하지 않습니다");

      if (getPaymentData.data.response.amount !== amount)
        throw new UnprocessableEntityException("잘못된 imp_uid입니다");

      return await getPaymentData.data.response;
    } catch (e) {
      if (e.response.status === 404) {
        throw new UnprocessableEntityException("유효하지 않은 impUid입니다.");
      }
      if (e?.response?.data?.message) {
        //아임포트 서버에서 받는 에러내용. 아임포트에러와 내가 만들어준 에러를 구분해서 써준다
        throw new HttpException(e.response.data.message, e.response.status);
      }
    }
  }
}
```

# 2. 결제 검증

[결제내역 단건조회 API](https://chai-iamport.gitbook.io/iamport/api/api-1/api-1)

<br>

`payment.resolver.ts` 에서는 `createPayment` 함수,

```ts

@UseGuards(GqlAuthAccessGuard)	//로그인한 유저만 가능하도록
  @Mutation(() => Payment)	//graphql설정
  async createPayment(  //결제 테이블에 데이터를 생성
    @Args('impUid') impUid: string,
    @Args({ name:'amount', type:() => Int }) amount : number, //그냥 number만 쓰게되면 1,000.0 이런식으로 소수점도 들어가기때문에 타입을 Int로 따로 또 한번 지정해주는게 필요하다. name은 amount로 쓰는대신 type은 Int로 하겠다는 타입지정이 한번 더 필요함.
    @Context() context: IContext,
  ): Promise<Payment> {

    //아임포트 서버에 결제완료 기록이 존재하는지 확인
    const token = await this.iamPortService.fetchToken( impUid );
    const paymentData = await this.iamPortService.fetchPaymentData({ impUid, token, amount });

    //impuid가 존재하는지 중복결과 체크
    await this.iamPortService.checkDuplicate({ impUid })

    const user = context.req.user;
    return this.paymentService.create({ impUid, amount, user , paymentData });
  }
```

<br>

`payment.service.ts` 에서는 `create` 함수가,

```ts
async create({ impUid, amount:_amount, user:_user, status = POINT_TRANSACTION_STATUS_ENUM.PAYMENT, paymentData } ): Promise<Payment> {   //status = POINT_TRANSACTION_STATUS_ENUM.PAYMENT은 디폴트값을 설정해주는 문법. 만약에 어디선가 status에 새로운 값이 있으면 새로운 값으로 넣어준다
    const payment = this.paymentsRepository.create({
      impUid,
      amount : _amount,
      user : _user,
      status,
    })

    //결제 데이터를 결제 테이블에 추가
    await this.paymentsRepository.save(payment);

    //user테이블에 동기화(누적금액으로)
    const user = await this.userRepository.findOne({ where : { id : _user.id } })
    console.log(`유저가 이미 결제한 내역은 ${user.paid}이며, 이번에 새로 결제한 금액은 ${_amount}입니다`)

    await this.userRepository.update({ id: user.id },{ paid: user.paid + _amount })

    //결제 데이터를 클라이언트에 리턴
    return payment;

  }
```

<br>

`iamport.service.ts` 는 `iamport.service.ts` 파일 안의 모든 내용이 결제 검증에 필요한 내용이다.

특히, `fetchPaymentData` 에서

```ts
async fetchPaymentData( { impUid : imp_uid, token, amount }: IPaymentsServiceFetchData){
    try{
      const getPaymentData = await axios({
        url: `https://api.iamport.kr/payments/${imp_uid}`,
        method: "get",
        headers: {
          "Content-Type": "application/json",
          "Authorization": token }
      });

      if(getPaymentData.data.response.status !== 'paid') throw new ConflictException('결제 내역이 존재하지 않습니다')

      if(getPaymentData.data.response.amount !== amount) throw new UnprocessableEntityException('잘못된 imp_uid입니다')

      return await getPaymentData.data.response;
    }catch(e){
      if (e.response.status === 404) {
        throw new UnprocessableEntityException('유효하지 않은 impUid입니다.');
      }
      if(e?.response?.data?.message){  //아임포트 서버에서 받는 에러내용. 아임포트에러와 내가 만들어준 에러를 구분해서 써준다
        throw new HttpException(
          e.response.data.message,
          e.response.status
        )
      }
    }

  }
```

<br>

axios로 받아오는

```ts
const getPaymentData = await axios({
  url: `https://api.iamport.kr/payments/${imp_uid}`,
  method: "get",
  headers: {
    "Content-Type": "application/json",
    Authorization: token,
  },
});
```

이 부분이 결제정보를 받아오는 api 부분인데 독스에 잘 써져 있다.

imp_uid를 파라미터로 넘겨야지 조회를 할 수 있기 때문에 무조건 imp_uid를 받아와야한다.

imp_uid를 받아오려면 token을 받아와야하기때문에

`const token = await this.iamPortService.fetchToken( impUid );`

이 함수와 함께 fetchToken 함수를 써준다.

<br>

# 3. 결제취소

[결제취소 API 독스](https://chai-iamport.gitbook.io/iamport/api/api-1/api)

<br>

```ts
//payment.resolver.ts

@UseGuards(GqlAuthAccessGuard)
  @Mutation(() => Payment)
  async cancelPayment(  //결제를 취소하는 API
    @Args('impUid') impUid: string,
    @Args({ name:'amount', type:() => Int }) amount : number,
    @Context() context: IContext,
  ): Promise<Payment> {


    //이미 취소된 건인지 확인
    await this.paymentService.checkAlreadyCanceled( impUid );

    //취소하기에 충분한 금액이 남아있는지
    const user = context.req.user;
    await this.paymentService.checkHasCancelableMoney({ impUid, user });

    //실제로 아임포트에 취소 요청하기
    const token = await this.iamPortService.fetchToken( impUid );
    const paymentData = this.iamPortService.fetchPaymentData({ impUid, token, amount });

    //결제테이블에 결제 취소 등록하기
    return await this.paymentService.cancelPaymentTable({ impUid, amount , user, paymentData });

  }
```

<br>

`payment.service.ts` 에서는 `checkAlreadyCanceled`, `checkHasCancelableMoney`, `cancelPaymentTable` 함수가 사용된다.

iamport.service.ts에서는 쓰인게 없다..

iamport.service.ts는 결제관련 함수만 모아져 있다.

<br>

<br>

<br>

<br>

<br>

---

# 최종코드 (결제 + 결제검증 + 결제취소)

<br>

```html
//payment.html

<!DOCTYPE html>
<html lang="ko">
  <head>
    <title>결제페이지</title>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <!-- jQuery -->
    <script
      type="text/javascript"
      src="https://code.jquery.com/jquery-1.12.4.min.js"
    ></script>
    <!-- iamport.payment.js -->
    <script
      type="text/javascript"
      src="https://cdn.iamport.kr/js/iamport.payment-1.2.0.js"
    ></script>
    <script>
      const requestPay = () => {
        const IMP = window.IMP; // 생략 가능
        IMP.init("impxxxxxxxxx"); //본인의 가맹점 번호
        // IMP.request_pay(param, callback) 결제창 호출
        IMP.request_pay(
          {
            // param
            pg: "nice", //이거 본인이 사용하는 pg사 이름 안써주면 안뜸(나는 nice를 쓰느라고 nice로 바꿈)
            pay_method: "card",
            // merchant_uid: "ORD20180131-0000011", //주문번호 겹치면 에러남(주석하면 랜덤으로 생성됨). imp_uid나 merchant_uid 둘중에 하나만 있으면 되서 이거는 주석처리함
            name: "스타벅스 텀블러",
            amount: 100,
            buyer_email: "gildong@gmail.com",
            buyer_name: "홍길동",
            buyer_tel: "010-4242-4242",
            buyer_addr: "서울특별시 강남구 신사동",
            buyer_postcode: "01181",
          },
          function (rsp) {
            // callback
            if (rsp.success) {
              axios.post(
                "http://localhost:3000/graphql",
                {
                  query: `
                mutation{
                  createPayment(
                    impUid : "${rsp.imp_uid}", amount: ${rsp.paid_amount}
                  ){
                    id
                  }
                }
              `,
                },
                {
                  headers: {
                    Authorization:
                      "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InFAcS5jb20iLCJzdWIiOiI0Y2I1Zjg3ZS0wNWU2LTQwNjMtOTcxYy0xMzUzOTY4Njc4MDQiLCJpYXQiOjE2Njk5NzQwNjgsImV4cCI6MTY2OTk3NzY2OH0.DiKyobAKVxiJYTwMyB_DcXBF1QdSiHvTv1c30vFBI0o",
                  },
                }
              );

              alert("결제 성공 🥳");
              rsp.impUid;
              rsp.amount;
            } else {
              alert("결제 실패 🥺");
            }
          }
        );
      };
    </script>
  </head>
  <body>
    <button class="btnClick" onclick="requestPay()">결제하기</button>
  </body>
</html>
```

<br>

```ts
//payment.resolver.ts

import { UseGuards } from "@nestjs/common";
import { Args, Context, Int, Mutation, Resolver } from "@nestjs/graphql";
import { GqlAuthAccessGuard } from "src/commons/auth/gql-auth.guard";
import { IContext } from "src/commons/types/context";
import { IamPortService } from "../iamport/iamport.service";
import { Payment } from "./entities/payment.entity";
import { PaymentService } from "./payments.service";

@Resolver()
export class PaymentResolver {
  constructor(
    private readonly paymentService: PaymentService,

    private readonly iamPortService: IamPortService //새로운 iamport.ts를 만들어서 iamport관련 함수들은 다 모아두기
  ) {}

  @UseGuards(GqlAuthAccessGuard) //로그인한 유저만 가능하도록
  @Mutation(() => Payment) //graphql설정
  async createPayment(
    //결제 테이블에 데이터를 생성
    @Args("impUid") impUid: string,
    @Args({ name: "amount", type: () => Int }) amount: number, //그냥 number만 쓰게되면 1,000.0 이런식으로 소수점도 들어가기때문에 타입을 Int로 따로 또 한번 지정해주는게 필요하다. name은 amount로 쓰는대신 type은 Int로 하겠다는 타입지정이 한번 더 필요함.
    @Context() context: IContext
  ): Promise<Payment> {
    //아임포트 서버에 결제완료 기록이 존재하는지 확인
    const token = await this.iamPortService.fetchToken(impUid);
    const paymentData = await this.iamPortService.fetchPaymentData({
      impUid,
      token,
      amount,
    });

    //impuid가 존재하는지 중복결과 체크
    await this.iamPortService.checkDuplicate({ impUid });

    const user = context.req.user;
    return this.paymentService.create({ impUid, amount, user, paymentData });
  }

  @UseGuards(GqlAuthAccessGuard)
  @Mutation(() => Payment)
  async cancelPayment(
    //결제를 취소하는 API
    @Args("impUid") impUid: string,
    @Args({ name: "amount", type: () => Int }) amount: number,
    @Context() context: IContext
  ): Promise<Payment> {
    //이미 취소된 건인지 확인
    await this.paymentService.checkAlreadyCanceled(impUid);

    //취소하기에 충분한 금액이 남아있는지
    const user = context.req.user;
    await this.paymentService.checkHasCancelableMoney({ impUid, user });

    //실제로 아임포트에 취소 요청하기
    const token = await this.iamPortService.fetchToken(impUid);
    const paymentData = this.iamPortService.fetchPaymentData({
      impUid,
      token,
      amount,
    });

    //결제테이블에 결제 취소 등록하기
    return await this.paymentService.cancelPaymentTable({
      impUid,
      amount,
      user,
      paymentData,
    });
  }
}
```

<br>

```ts
//payment.service.ts

import { Injectable, UnprocessableEntityException } from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm";
import { DataSource, Repository } from "typeorm";
import { User } from "../users/entities/user.entity";
import {
  Payment,
  POINT_TRANSACTION_STATUS_ENUM,
} from "./entities/payment.entity";
import {
  IPaymentsServiceCreate,
  IPaymentsServiceCancel,
} from "./interface/payment-service.interface";

@Injectable()
export class PaymentService {
  constructor(
    @InjectRepository(Payment)
    private readonly paymentsRepository: Repository<Payment>,

    @InjectRepository(User)
    private readonly userRepository: Repository<User>,

    private readonly dataSource: DataSource
  ) {}

  //결제 테이블에 결제 내역 생성
  async create({
    impUid,
    amount: _amount,
    user: _user,
    status = POINT_TRANSACTION_STATUS_ENUM.PAYMENT,
    paymentData,
  }): Promise<Payment> {
    //status = POINT_TRANSACTION_STATUS_ENUM.PAYMENT은 디폴트값을 설정해주는 문법. 만약에 어디선가 status에 새로운 값이 있으면 새로운 값으로 넣어준다
    const payment = this.paymentsRepository.create({
      impUid,
      amount: _amount,
      user: _user,
      status,
    });

    //결제 데이터를 결제 테이블에 추가
    await this.paymentsRepository.save(payment);

    //user테이블에 동기화(누적금액으로)
    const user = await this.userRepository.findOne({ where: { id: _user.id } });
    console.log(
      `유저가 이미 결제한 내역은 ${user.paid}이며, 이번에 새로 결제한 금액은 ${_amount}입니다`
    );

    await this.userRepository.update(
      { id: user.id },
      { paid: user.paid + _amount }
    );

    //결제 데이터를 클라이언트에 리턴
    return payment;
  }

  async checkAlreadyCanceled(impUid) {
    const alreadyPaid = await this.paymentsRepository.findOne({
      where: { impUid: impUid, status: "CANCEL" },
    });
    // 이미 취소된 결제라면 UnprocessableEntityException 에러
    if (alreadyPaid)
      throw new UnprocessableEntityException("이미 결제가 취소되었습니다");
  }

  async checkHasCancelableMoney({ impUid, user: _user }) {
    const hasMoney = await this.paymentsRepository.findOne({
      where: {
        impUid: impUid, //
        user: { id: _user.id }, //user가 현재 로그인한 유저인지(다른 사람이 충전했을수도 있으니)
        status: POINT_TRANSACTION_STATUS_ENUM.PAYMENT,
      },
    });
    if (!hasMoney)
      throw new UnprocessableEntityException("결제 기록이 존재하지 않습니다");

    const user = await this.userRepository.findOne({ where: { id: _user.id } });
    if (user.paid < hasMoney.amount)
      throw new UnprocessableEntityException(
        "환불 요청 금액이 결제 금액보다 많습니다"
      );
  }

  async cancelPaymentTable({ impUid, amount, user: _user, paymentData }) {
    const payment = await this.create({
      impUid, //
      amount: -amount,
      user: _user,
      status: POINT_TRANSACTION_STATUS_ENUM.CANCEL,
      paymentData,
    });

    return payment;
  }
}
```

<br>

```ts
//iamport.service.ts

import {
  ConflictException,
  HttpException,
  Injectable,
  UnprocessableEntityException,
} from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm";
import axios from "axios";
import { Repository } from "typeorm";
import { resourceLimits } from "worker_threads";
import { Payment } from "../payments/entities/payment.entity";
import { IPaymentsServiceFetchData } from "../payments/interface/payment-service.interface";
import { User } from "../users/entities/user.entity";

@Injectable()
export class IamPortService {
  constructor(
    @InjectRepository(Payment)
    private readonly paymentsRepository: Repository<Payment>
  ) {}

  async checkDuplicate({ impUid }) {
    const result = await this.paymentsRepository.findOne({
      where: { impUid: impUid },
    });
    if (result) throw new ConflictException("이미 결제가 완료되었습니다");
  }

  //결제정보 조회하기
  async fetchToken(impUid: string): Promise<string> {
    try {
      //아임포트에서 토큰을 받아오기
      const getToken = await axios({
        url: "https://api.iamport.kr/users/getToken",
        method: "post", // POST method
        headers: { "Content-Type": "application/json" },
        data: {
          imp_key: process.env.IAMPORT_KEY,
          imp_secret: process.env.IAMPORT_SECRET,
        },
      });

      const access_token = await getToken.data.response.access_token; // 인증 토큰
      return access_token;
    } catch (e) {
      console.log(e);
      throw new HttpException(e.response.data.message, e.response.status);
    }
  }

  //결제정보 아임포트에서 가져오기(원두쌤checkpaid와 같음 함수)
  async fetchPaymentData({
    impUid: imp_uid,
    token,
    amount,
  }: IPaymentsServiceFetchData) {
    try {
      const getPaymentData = await axios({
        url: `https://api.iamport.kr/payments/${imp_uid}`,
        method: "get",
        headers: {
          "Content-Type": "application/json",
          Authorization: token,
        },
      });

      if (getPaymentData.data.response.status !== "paid")
        throw new ConflictException("결제 내역이 존재하지 않습니다");

      if (getPaymentData.data.response.amount !== amount)
        throw new UnprocessableEntityException("잘못된 imp_uid입니다");

      return await getPaymentData.data.response;
    } catch (e) {
      if (e.response.status === 404) {
        throw new UnprocessableEntityException("유효하지 않은 impUid입니다.");
      }
      if (e?.response?.data?.message) {
        //아임포트 서버에서 받는 에러내용. 아임포트에러와 내가 만들어준 에러를 구분해서 써준다
        throw new HttpException(e.response.data.message, e.response.status);
      }
    }
  }
}
```

<br>

<br>

<br>

<br>

<br>

<br>
