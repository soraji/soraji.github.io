---
layout: post
title:  "[Vue.js] Vue 부모컴포넌트와 자식컴포넌트의 데이터 송신방법"
categories: front 
comments: true

---



# 상위컴포넌트와 하위컴포넌트의 데이터 송신방법

```vue
//상위컴포넌트 html
<table class="tableclassnon"  v-for="(cat,i) in this.$store.state.cats">
    <tr>
    	<td class="blueText">{{cat.name}}</td>    
    	<td><detail :pcat="cat" :pIndex="i+1"></detail></td>
    </tr>
    <tr>
    	<td colspan="2" class="blueText" >{{cat.age}}</td>
    </tr>
</table>

```



for문에서의 cats는 vuex, store폴더에 있고

detail이라는 이름의 하위컴포넌트를 등록해준다.

```js
//상위컴포넌트 js
import detail from './detail.vue';	//같은 레벨의 detail컴포넌트를 가져온다

export default {
  components:{
    detail	//컴포넌트에 detail컴포넌트를 등록해준다. html에서 사용하려면 <detail></detail>로 사용한다.
  },
```



------

```vue
//하위컴포넌트 html
<input type="text" name="" id="" v-model="pcat.name">
<!-- input타입이기 때문에 값(value)를 v-model로 바인딩해준다-->
```

```js
//하위컴포넌트 js
export default {
  props:['pcat','pIndex'],	//props로 파라미터2개받기
```





근데 정말 숙달될때까지 너무너무너무너무 헷갈린다.

그래서 정리해봤는데 이해안가는 사람들한테 도움이 됐음 좋겠다

![](/assets/img/propsevent.jpg)