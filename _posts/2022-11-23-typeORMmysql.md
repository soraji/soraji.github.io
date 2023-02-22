---
layout: post
title: "[ GraphQL ] GraphQL with MySQL"
categories: back
comments: true
---

<br>

<br>

![typeorm](/assets/img/devcate/typeorm.png)

# TypeORM & Entity

엔티티 : ''개체'' 의 뜻을 가지고 있다.

엔티티를 설정해주면 mysql과 연결해서 사용할때 테이블과 컬럼들을 자동 생성해준다(무려 조인문까지도...!)

그래서 ERD로 그렸던 도식화를 이 엔티티로 연결해주면 mysql에서 테이블과 관계도 등등 자동으로 생성이되고

최종적으로는 mysql 엔티티에서 만들어진 ERD를 확인할 수 있다.

그렇게 내가 만든 ERD와 코드로 짜서 만들어진 db테이블관계간 ERD를 확인하고

내가 제대로 만들었는지 확인하면 보다 더 정확한 ERD를 만들 수 있다

<br>

<br>

그러면 테이블간 관계를 고려해서 엔티티를 만들어야하는데

테이블 관계는 `1:1`, `1:N`, `N:M` 가 있다.

---

## 1:1 테이블

중고상품과 중고상품을 거래하는 위치는 1:1관계이다(라고 가정해보자)

```js
@Entity()
export class Product {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  name: string;

  @Column()
  isSoldout: boolean;

  @JoinColumn()
  @OneToOne(() => ProductSaleslocation) ////1:1관계. 모든 상품의 장소는 다르기 때문에 1:1(실제 기획에서는 아닐수도있지만 여기서는 모두 다 다르다고 가정하고 진행)
  productSaleslocation: ProductSaleslocation;
}
```

- `@Entity` : class가 실행될 때, typeorm에 의해 Entity 테이블을 만들어 준다.

- `@PrimaryGeneratedColumn(' ')` : 자동으로 생성될 값의 컬럼.

  - `increment` : 숫자로 데이터가 쌓일때마다 **숫자가 하나 하나씩 올라가는 PK 키**를 만들 수 있습니다. 테이블에 반드시 있어야하는 유일값이다.
  - `uuid` ( = Universal Unique IDentifier ) : 중복되지 않는 **고유한 PK 키**를 만들 수 있음

- `@Column({ type : ‘text’ })` : ERD에서 타입을 지정해주었는데, 엔티티에서 **타입**을 원하는대로 **지정**해 줄 수 있다.

  정해주지 않으면( 빈 괄호로 둘 경우) default 값으로 들어가게 되는데, 요즘은 잘 써주지 않는다고 한다.
  만약 빈 값을 입력받아도 된다고 하면 `{ nullable: true }` 를 적어준다

- `@OneToOne()` : 두 테이블의 관계를 나타내는 것으로 @OneToOne( ) 은 **한쪽에만** 쓰거나, **양쪽에 모두** 써줄 수 있다

- `@JoinColumn()` : 두 테이블을 하나로 합쳐서 데이터를 가져와야하기에 사용하였으며, **한쪽 테이블에만** 적어줘야 함

  (이걸 잘못보고 나는 처음에 `JoinTable()` 사용해서 도대체 뭐가 문제인지 발견 못했음;;; 🫠 조인 테이블이 아니구 조인 컬럼임!)

<br>

<br>

---

## 1:N 테이블

상품과 상품카테고리 관계를 생각해보자. 상품카테고리에는 상품이 여러개 들어갈 수 있다

그러니까 상품카테고리(1)와 상품(N)은 1:N 관계이다

```js
// productCategory.entity.ts (상품카테고리 테이블이 될 엔티티. 1이 된다)

import { Column, Entity, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class ProductCategory {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  name: string;
}
```

```js
// product.entity.ts (상품 테이블이 될 엔티티. N이 된다.)

import { ProductCategory } from "src/apis/productsCategory/entities/productCategory.entity";
import { ProductSaleslocation } from "src/apis/productsSaleslocation/entities/productSaleslocation.entity";
import {
  Column,
  Entity,
  JoinColumn,
  ManyToOne,
  OneToOne,
  PrimaryGeneratedColumn,
} from "typeorm";

@Entity()
export class Product {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  name: string;

  @Column()
  isSoldout: boolean;

  @JoinColumn() ////join의 기준이 되는 컬럼이다
  @OneToOne(() => ProductSaleslocation) //1:1관계에서 했던 장소와 1:1관계를 가지고 있음
  productSaleslocation: ProductSaleslocation;

  //1:N관계에서 현재 이 테이블이 N이므로 Many가 앞에 쓰인다. 자기자신을 기준으로 먼저 쓰게 되니까..다(상품)대일(상품카테고리) 관계.
  //컬럼이면서 연결되는 컬럼(단순컬럼이 아님). 어떤 관계가 있는지 알려줘야함
  //'ProductCategory가 One이야' 라고 알려줌
  @ManyToOne(() => ProductCategory)
  productCategory: ProductCategory;
}
```

- `@ManyToOne()` : N : 1 관계를 나타내는 데코레이터.

- @JoinColumn()

  Many 부분에 해당하는 테이블(product)에서는 JoinColumn( ) 이 생략 가능

  - **@ManyToOne(** ) : @JoinColumn( ) **생략가능**
  - **@OneToOne( )** : @JoinColumn( ) **반드시 필요**

<br>

<br>

---

## N:M 관계

상품과 상품태그 테이블은 N:M 관계

상품 하나는 여러개의 태그를 가질 수 있고, 태그도 하나의 태그가 여러 상품에 사용될 수 있다 (어우 복잡)

이렇게 N:M 일 경우에는 상품\_상품태그 테이블 처럼 중간 테이블 을 만들어 사이에 두고 각각 1:N의 관계를 갖도록 그려주는데,

엔티티에서는 따로 테이블을 만들어주는게 아니라 자동으로 생성해준다

```js
// prodcutTag.entity.ts ( 상품태그 테이블이 될 엔티티. 여기서는 N)

import { Product } from "src/apis/products/entities/product.entity";
import { Column, Entity, ManyToMany, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class ProductTag {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  name: string;

  //ManyToMany는 양쪽에 다 적어주어야 함. 1:1은 아무쪽 한곳에다만 적으면 되고, 1:N은 N에다만 적어주면 되는데 N:M은 양쪽이 서로가 어떻게 생각하는지 적어줘야한다
  @ManyToMany(() => Product, (products) => products.productTags)
  products: Product[];
}
```

- `(productTags) => productTags.products` : productTags 입장에서의 prodcuts 와의 관계를 명시해 준 것으로,

  N : M 관계에서는 두 테이블 모두 관계를 나타내 주어야함

- `ProductTag[]` : 하나의 상품이 여러개의 태그에 해당 될 수 있기에 배열로 나타내는 것

```js
// prodcut.entity.ts ( 상품 테이블이 될 엔티티. 여기서는 M)

import { ProductCategory } from "src/apis/productsCategories/entities/productCategory.entity";
import { ProductSaleslocation } from "src/apis/productsSaleslocations/entities/productSaleslocation.entity";
import { ProductTag } from "src/apis/productsTags/entities/productTag.entity";
import { User } from "src/apis/users/entities/user.entity";
import {
  Entity,
  Column,
  PrimaryGeneratedColumn,
  ManyToOne,
  OneToOne,
  JoinColumn,
  ManyToMany,
  JoinTable,
} from "typeorm";

@Entity()
export class Product {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  name: string;

  @Column() //단순컬럼
  isSoldout: boolean;

  @ManyToOne(() => ProductCategory) //컬럼이면서 연결되는 컬럼(단순컬럼이 아님). 어떤 관계가 있는지 알려줘야함
  //'ProductCategory가 One이야' 라고 알려줌
  productCategory: ProductCategory;

  @ManyToOne(() => User)
  user: User;

  @JoinColumn() //join의 기준이 되는 컬럼이다
  @OneToOne(() => ProductSaleslocation) //1:1관계. 모든 상품의 장소는 다르기 때문에 1:1(실제 기획에서는 아닐수도있지만 여기서는 모두 다 다르다고 가정하고 진행)
  productSaleslocation: ProductSaleslocation;

  //상품과 상품태그는 1:M. 상품1개에 여러개의 태그가 달릴수있다 (여러개니까 배열). productTag.entitiy.ts 파일안에도 @MantToMany를 붙여주면 DB에서 컬럼으로 생각하지 않고 아예 다르게 작동시킨다. 자동으로 table을 생성한다.
  //앞의 () => ProductTag는 '나'인 product table이고, 내 입장에서 나는 productTag를 가리키고 있어. 근데 상대편인 productTag는 나(product)를 어떻게 생각하는데? 라고 물어봄.
  @JoinTable()
  @ManyToMany(() => ProductTag, (productTags) => productTags.products)
  productTags: ProductTag[];
}
```

- `@JoinTable()` : N : M 관계에서 생성되는 **중간 테이블을 자동으로 만들어 주는 것**으로 기준이 되는 테이블 한 쪽에만 작성

- `@ManyToMany()` : N :M의 관계를 가질 때는 두 테이블 모두 컬럼을 추가하여 연결

- `(products) => products.productTags` : products 입장에서의 productTags 와의 관계를 명시해 준 것으로,

  N : M 관계에서는 두 테이블 모두 관계를 나타내 주어야함.

- `Product[]` : 하나의 태그에 상품이 여러개 해당될 수 있기에 배열로 나타내는 것.

<br>

<br>

---

# mysql 명령어

데이터베이스 조회

```
show databases
;

use myproject
;

show tables
;

desc product
;

```

<br>

<br>

---

product 테이블 전체(\*) 조회

```
select *
	from product
;
```

<br>

<br>

---

insert문 : `insert into 테이블먕(컬럼1, 컬럼2, 컬럼3) values (값1, 값2, 값3)`

```
insert into product(id, name, description, price, isSoldout)
	values(uuid(), '마우스','정말 좋은 마우스임', 15000, false)
;

insert into product(id, name, description, price, isSoldout)
	values(uuid(), '노트북','맥북샀음 ㅜ', 150000, false)
;

insert into product(id, name, description, price, isSoldout)
	values(uuid(), '셔츠','깨끗한셔츠지롱', 150000, false)
;
```

<br>

<br>

---

delete문 : `delete from 테이블명 where 조건`

```
delete from product
	where name = '셔츠'
;
```

<br>

<br>

---

update문 : `update 테이블명 set 바꿔주고 싶은 내용 where 조건`

```
update product
	set price = 18000
	where name = '마우스'
;
```

<br>

<br>

---

Select문 : 전체 조회

```
select *
	from product_saleslocation
;
```

<br>

<br>

---

insert문 : `insert into 테이블먕(컬럼1, 컬럼2, 컬럼3) values (값1, 값2, 값3)`

```
insert into product_saleslocation(id, address, addressDetail, lat, lng, meetingTime)
	values(uuid(), '구로구', '구로디지털단지', 34.987640, 127.354749, '2022-12-25')
;
```

<br>

<br>

---

update문 : `update 테이블명 set 바꿔주고 싶은 내용 where 조건`

```
update product
	set productSaleslocationId = '60308f8e-6b06-11ed-b524-1560a20f62c0'
	where name = '마우스'
;
```

<br>

<br>

---

select문

```
select p.id, name, price, address, addressDetail
	from product p, product_saleslocation ps
where p.productSaleslocationId = ps.id
;
```

- from product, product_saleslocation이 2개의 테이블을 join할거라는 말(`,`로 연결함)

- product p => product를 p로 사용하겠다

- select p.id, name, price, address, addressDetail as '상세주소' 로하게되면 컬럼명이 '상세주소'로 바뀜

- mysql은 더블쿼테이션이 인식되지만 다른것들과의 통일성을 위해 그냥 싱글쿼테이션을 사용하도록 하자

<br>

<br>

---

2개 이상의 그리고 조건 : `and`

```
#추가기능들 -1(앤드 명령어 )
update product
	set isSoldout = TRUE
	WHERE name = '노트북'
	AND price = 20000
;
```

<br>

<br>

---

2개 이상의 또는 조건 : `OR`

```
#추가기능들 -2(또는 명령어 )
update product
	set isSoldout = TRUE
	WHERE name = '노트북'
	OR name = '키보드'
;
```

<br>

<br>

---

주석은 `--` 로 한다

```
#추가기능들 -3(명령문 주석)
SELECT *
FROM product p
WHERE location = '구로'
-- AND price = 5000
AND isSoldout  = FALSE
```

<br>

<br>

---

where안에 AND/OR을 개행해서 넣게 되면 주석 처리할때 편하다

근데 delete나 update문에서는 웬만하면 사용하지 말기

왜냐면 주석처리가 만약에 전체가 다 되어버리면 전체 삭제 혹은 전체 업데이트가 되기 때문에 너무 위험...

```
#추가기능들 -4(주석 쉽게 다는 방법 )
SELECT *
FROM product p
WHERE 1 = 1	#무조건 참이기 때문
	AND location = '구로'
	-- AND price = 5000
	AND isSoldout  = FALSE
```

<br>

<br>

<br>

<br>

<br>
