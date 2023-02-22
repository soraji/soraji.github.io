---
layout: post
title: "[ Node.js ] TypeScript Type(타입스크립트 타입: 유틸리티 타입/제네릭 타입)"
categories: back
comments: true
---

<br>

<Br>

![typescript](/assets/img/devcate/ts.png)

# TypeScript Type

<br>

타입스크립트에는 `유틸리티 타입`과 `제네릭 타입` 의 타입이 있다.

우선 처음으로는 유틸리티 타입을 먼저 보도록 하자.

<br>

<br>

## Utility Types(유틸리티 타입)

- 공통 타입 변환을 편리하게 하기 위해 몇 가지 유틸리티 타입을 제공
- Utility Type은 기존에 있던 Type들을 변형해서 변형된 타입을 새로 만들어주는 역할
- 같은 코드를 여러번 작성하지 않기 위해 많이 사용

유틸리티 타입의 확인을 위해 임의의 타입을 하나 지정

```ts
interface IProfile {
  name: string;
  age: number;
  school: string;
  hobby?: string;
}
```

<Br>

<br>

### Partial Type : 모든 속성을 선택사항으로 바꿔주는 역할

```ts
//1.partial 타입
type aaa = Partial<IProfile>;
// type aaa = {
//   name?: string;
//   age?: number;
//   school?: string;
//   hobby?: string;
// }
```

### Required Type : 모든 속성을 필수사항으로 바꿔주는 역할

```
//2.Required 타입
type bbb = Required<IProfile>
// type bbb = {
//   name: string;
//   age: number;
//   school: string;
//   hobby: string;
// }
```

### Pick Type : 원하는 속성만을 뽑아서 사용

```
//3.pick 타입
type ccc = Pick<IProfile, "name" | "age">
// type ccc = {
//   name: string;
//   age: number;
// }
```

### Omit Type : 원하는 속성만 제거하여 사용

```ts
//4.Omit 타입
type ddd = Omit<IProfile, "school">;
// type ddd = {
//   name: string;
//   age: number;
//   hobby?: string;
// }
```

### Record Type : Utility Type 속성을 다른 Type으로 매핑

```ts
// 5. Record 타입
type MyUnion = "철수" | "영희" | "훈이"; // Union 타입
type MyType5 = Record<MyUnion, IProfile>;

let child: eee;
child = "바나나"; //철수, 영희, 훈이 3개중에서만 사용할 수 있음

type fff = Record<eee, IProfile>;
// type fff = {
//   철수: IProfile;
//   영희: IProfile;
//   훈이: IProfile;
//}
```

<Br>

<br>

> ❓ **Union Type**
>
> : Javascript의 OR 연산자( `||` )와 같이 **‘A’ 이거나 ‘B’이다** 라는 의미의 타입으로,
>
> `**|**` 연산자를 이용하여 **타입 또는 값을 여러 개 연결할 때 사용**
>
> ```ts
> type MyUnion = "철수" | "영희" | "훈이"; // Union 타입
> let child: MyUnion;
> child = "철수";
> ```

<Br>

<br>

> ❓ **Keyof Type**
>
> 해당 객체 내 key값을 Union 형태로 반환시켜 주는 역할
>
> 즉, IProfile의 key만을 뽑아서 새로운 타입을 만들고 싶을때 사용함
>
> ```ts
> interface IProfile {
>   name: string;
>   age: number;
>   school: string;
>   hobby?: string;
> }
>
> let mykey: keyof IProfile; //  "name" | "age" | "school" | "hobby" 와 같음
> mykey = "name";
> ```

<Br>

---

## Generic Types(제네릭 타입)

- 재사용성이 높은 컴포넌트를 만들 때 자주 활용되는 특징
- 한가지 타입보다 여러 가지 타입에서 동작하는 컴포넌트를 생성하는데 사용

<br>

<Br>

<br>

<Br>

<br>

<Br>
