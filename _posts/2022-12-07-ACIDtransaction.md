---
layout: post
title: "[ CS ] ACID transaction (ACID트랜잭션)"
categories: back
comments: true
---

# ACID transaction

DB에 정보를 집어넣다보면 정말 0.000001초 찰나에 데이터가 꼬이게 될 가능성이 있다.

예를들어 내가 a라는 명령을 했는데, 거기에 연결되어있는 데이터값을 누군가가 수정을 하는바람에 데이터가 꼬이게 되면

그 자체로 데이터가 오염이 되어버린다

사실 굉장히 큰 문제인것이다. 특히 결제쪽으로 만약 이런 문제가 발생하게된다면... 어익후야 (;´༎ຶД༎ຶ`)

<br>

<br>

그래서 이런 데이터 오염을 막아줘야한다.

이때 등장하는개념이 바로 트랜잭션이라는 개념인데,

트랜잭션은 어떤 한 기능을 완료하기 위해 수행되는 전체적인 하나의 프로세스를 뜻한다.

이렇게 말하니까 어려우니까.. 쉽게 설명하자면

'xx아 편의점가서 라면 하나 사와' 라고 명령을 내리면

xx가 편의점을 가서 라면을 산뒤에 심부름을 시킨 나에게 돌아오는것까지가 하나의 트랜잭션이 되는 것이다.

근데 만약에 이 중간에 어떤 일이 생겨서 편의점에 가지 못하거나, 아니면 라면이 품절이거나, 아니면 나에게 돌아오지 못하거나

이런식의 문제가 발생할 수 있다.

그러니까

1. 트랜잭션을 처리하는데
2. 모든 트랜잭션이 완료가 되면 commit을 하고
3. 만약 하나라도 중간에 에러가 나면 초기에 수행했던 모든 작업들을 취소시키면서 rollback을 해주는것이다.

문제가 중간에 발생하면 그 문제에 대한 해결방안을 처리해주어야하는데 그때 사용하는 개념이

`queryRunner` 라는 것이다.

Nest.js에서는 쿼리러너를 권장하는데 이유는 commitrhk rollback을 수동으로 제어가 가능하기 때문이라고 한다

(Jest를 통해 mocking을 좀 더 쉽게 도와주고 unit test가 더 쉬워진다는 장점이 있다고 한다)

<br>

<br>

repository는 `@InjectRepository` 데코레이터로 의존성을 주입하는데

queryRunner는 typeORM 모듈을 import해오는것만으로도 의존성이 주입되는것이라 따로 contructor에 데코레이터를 써줄필요가 없다

```js
const queryRunner = this.dataSource.createQueryRunner();
await queryRunner.connect();
await queryRunner.startTransaction();
```

`createQueryRunner` : 쿼리러너 선언

`connect` : DB연결

`startTransaction` : 트랜잭션의 시작을 선언한다. (시작과 끝을 제어할 수 있음)

<br>

<br>

```js
//레포에 save
await this.pointsTransactionsRepository.save(pointTransaction);

//쿼리러너로 save
await queryRunner.manager.save(pointTransaction);
```

그리고 원래는 위에처럼 레포에 저장하는 방식이었는데,

트랜잭션으로 검증하기 위해서는 `manager.save` 를 이용한다.

manager.save를 해주어야 '야 트랜잭션. 너가 관리해야하는 애야!' 라고 알려주는것임

만약 manger.save로 안쓰면 트랜잭션이 자기가 관리하지 않아도 되는 대상인줄 알고 에러가 나도 그냥 넘어가게 된다

<br>

<br>

그러니까 정리하면,

모든 트랜잭션이 성공하면 `commitTransaction()` 으로 commit을 확정짓고

하나라도 에러가나면 `rollbakTransaction()` 으로 rollback수행을 해준다.

그리고 finally에서는 무조건 `release()` 로 transaction을 종료시켜주어야 한다.

만약 트랜잭션을 종료시켜주지 않으면 연결누적으로 인해 DB접속이 불가능해진다(그러니까 release꼭 해주라구... ヽ(ﾟ ◇ ﾟ )ﾉ. )

만약에 누적된 연결들로 DB접속이 안되면 DB에서 자고있는 애들을 `kill 아이디` 로 죽여줘야한다... ୧༼◔ 益 ◔୧ ༽

그때 사용하는 mysql 쿼리명령문

```mysql
# DB보여줘
show databases;

# DB 변경해줘
use myproject;

# 테이블 보여줘
show tables;

# 커넷견 최대값(max_connections)
show variables;

# 커넥션 최댓값 조정
set global max_connections = 15;

# 현재 연결된 커넥션 갯수(thread_connection
show status;

# 현재 연결된 커넥션 목록
show processlist;



# 강제종료
kill 아이디;
```

<br>

<br>

<br>

아, 그리고

ACID는 외워야하는건 아니지만 한번 들어놓으면 좋을 듯

- `A(Atomicity)` : 원자성

  → 안전성 보장을 위해 가져야 할 성질 중의 하나로 트랜잭션과 관련된 작업들이 부분적으로 실행되다가 중단되지 않는 것을 보장

하는 능력.

모두 성공할 것 아니면 모두 실패하게 만드는 것(DB의 오염을 막기 위함).

- `C(Consistency)` : 일관성

  → 트랜잭션이 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태로 유지하는 것.

  - 똑같은 쿼리를 조회할 때마다 동일한 결과값이 나타나야하는 것.
  - 수정과 삭제로 인해 결과값이 달라지는 것은 당연함.

- `I(Isolation)` : 격리성

  → 트랜잭션을 수행 시 다른 트랜잭션의 연산 작업이 끼어들지 못하도록 보장하는 것.

  - A 사람의 요청을 처리하는 동안 B사람의 요청은 잠시 기다리는 것

- `D(Durability)` : 지속성

  → 성공적으로 수행된 트랜잭션에 대한 로그가 남아야하는 성질로 런타임 오류나 시스템 오류가 발생하더라도, 해당 기록은 영구적이어야 하는 것.

- 성공하여 commit이 되었으면 서버를 다시 켜도 그 데이터는 그대로 유지가 되어야 되는 것.

---

# Isolation-level (: 트랜잭션의 격리수준)

<br>

- ## Read uncommitted

  - 권장하지 않는다.
  - Dirty read 문제가 발생한다
    - Dirty read란? 트랜잭션 작업이 완료가 안됐는데 다른 트랜잭션에서 조회가 가능한 문제

---

- ## Read committed

  - RDB에서 대부분 기본적으로 사용한다. (2,3단계를 기본으로 사용함)
  - Dirty read 문제는 없다.
  - NON REPEATABLE READ문제가 발생한다.
    - NON REPEATABLE READ란? 결과적으로 동일한 쿼리문인데 결과값이 다르게 나타나는 문제
  - Repeatable read의 정합성(하나의 트랜잭션 내에 똑같은 select쿼리 실행시 항상 같은 결과값이 나와야함) 에 어긋남

---

- ## repeatable read

  - RDB에서 대부분 기본적으로 사용한다. (2,3단계를 기본으로 사용함)
  - Phantom read문제가 발생한다. (근데 다행인건 mysql에서는 자동으로 phantom read를 제거해주는 기능이 있어서 신경안써도됨)
    - Phantom read란 ? 종종 다른 트랜잭션에서 수행한 변경작업으로 레코드가 보였다 안보였다 하는 문제
    - Phantom read를 방지하기 위해 SERIALIZABLE을 사용
    - SERIALIZABLE : 동시처리성능 가장 안좋음... 가장 엄격. 사용 잘 안함

---

- ## serialable

  - 잘 사용 안함.
  - `Lock` 을 걸 수 있음 (트랜잭션 수행중에는 쿼리가 끝날때까지 다른 데이터를 건드릴 수 없음)
  - 성능이 느려지므로 꼭 필요한 곳에서만 사용해야함
