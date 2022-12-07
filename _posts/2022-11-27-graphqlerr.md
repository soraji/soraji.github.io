---
layout: post
title:  "error)UnhandledPromiseRejectionWarning: Error: Cannot determine a GraphQL output type for the 'x'. Make sure your class is decorated with an appropriate decorator."
categories: error
comments: true

---

<br>

<br>

# UnhandledPromiseRejectionWarning: Error: Cannot determine a GraphQL output type for the 'x'. Make sure your class is decorated with an appropriate decorator.

<br>

<br>

graphql에서 필드를 사용하기 위해서는 엔티티에 Feild를 지정해줘야하는데 없어서 에러난거임

~~~
//xxx.entity.ts

import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class ProductCategory {
  @PrimaryGeneratedColumn('uuid')
  category_id: string;

  @Column()
  category: string;
}
~~~

이렇게 되어있는 애들을 import도 해주고, @Field 제너레이터를 넣어주면 사라짐!

<br>

~~~
//xxx.entity.ts

import { Field, ObjectType } from '@nestjs/graphql';
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
@ObjectType()
export class ProductCategory {
  @PrimaryGeneratedColumn('uuid')
  @Field(() => String)
  category_id: string;

  @Column()
  @Field(() => String)
  category: string;
}

~~~



<br>

<br>
