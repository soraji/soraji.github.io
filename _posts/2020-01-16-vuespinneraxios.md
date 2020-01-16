---
layout: post
title:  "[Vue.js] vue spinner axios"
categories: front 
comments: true
---

vue-spinner기능을 이용해서 원하는 spinner를 사용했다.

나는 [vue-spinner](https://www.npmjs.com/package/vue-spinner)를 사용했다.

~~~
$ npm install vue-spinner
~~~



github에 들어가있으면 자세하게 나와있어서, 사용하는데는 편했는데

문제는 맨처음에 로딩할때만 spinner가 뜨고, axios를 사용할때는 뜨지 않았다

axios가 빠르긴하지만(?), 그래도 한 0.5초 정도 한템포가 느린데, 그 사이에 spinner가 돌면 좋겠다 싶었다.

axios로 데이터를 받을때 모두 spinner가 뜨게 하는 방법은!



생각보다 너무 쉬워서 허무했댱

~~~javascript
dispatch(함수명,{payload});
this.$store.state.loading = true;
~~~

이렇게 써주면된다. 난 vuex에서 써줬기 때문에 store이지만

만약 data에 선언했다면

~~~
this.loading = true;
~~~

로 적어주면 바로 적용된다. 유후