---
layout: post
title:  "vue input value에 공백제거, 숫자만 입력가능하게 하기"
categories: vue 
comments: true
---



## input value에 공백 없애기

~~~
val.replace(/\s/g,'')
~~~



---

## input value에 숫자만 입력가능하게 하기

~~~
onKeyup="this.value=this.value.replace(/[^0-9]/g,'');"
~~~



---