---
layout: post
title:  "[Vue.js] for문에서 child 자식컴포넌트에 파라미터 던지기"
categories: front 
comments: true


---







for문에서 예를들어 20개의 리스트를 뿌리고 그 리스트를 누르면 각각 상세페이지로 가는 페이지를 구현하려고 한다.



처음에 detail컴포넌트를 for문 안에 넣으니 컴포넌트가 20개가 동시에 생겼다.

그리고 가장 마지막에 가져온 20번째의 컴포넌트가 항상 최상단에 떠서 밑에 있는 내용들은 볼수가 없었다.

결국은 한건씩 정보를 가지고 와서 한번의 detail컴포넌트만 띄워야 했다. 1번을 클릭하면 1번의 내용이 뜨고, 20번을 클릭하면 20번의 내용만 뜨도록..

그래서 html에서는 for문에 넣어주지 않고 저 밑에다 한번만 불러오도록 적어준다.



```vue
//상위컴포넌트 html
<!-- for문 table에 걸어줌. konggos(배열)에 있는것 하나씩 꺼냄-->
<table v-for="(cat,i) in this.$store.state.cats" class="tableContent">
    <tr>
	    <td class="blueText" v-on:click="$refs.childref.show(cat,i+1)" >{{cat.number}}</td>
    </tr>
</table>
<!-- for문안에 적지 않는다 -->
<detail ref="childref"></detail>

//상위cmp js
import detail from './detail.vue';	//하위cmp 가져오기

export default {
  components:{
    detail	//cmp등록
  }
}
```

상위컴포넌트에서 하위컴포넌트의 methods를 바로 컨트롤 하려면 refs를 사용하면 된다.

```vue
v-on:click에 $refs.하위컴포넌트ref이름.메소드명(변수)
```

라고 적어주면 td를 클릭할때마다 하위컴포넌트에 있는 methods중의 show메소드에 접근한다.

정보가 담겨있는 cats(배열)과 index를 같이 변수로 넘겨준다.

```js
//하위컴포넌트(detail.vue)
<input type="text" name="" id="" v-model="index" > <!-- input은 value를 적어주어야해서 v-model로 바인딩-->
<div>{{cats.number}}</div>



export default {
  data(){
    return {
      cats:[],
      index:[]
    }
  },
  methods:{
    show:function (cat,index) {
      var vm = this;
      vm.cats = cat;
      vm.index = index;
    }
  }
}
```

data는 

처음에 data를

```
data:{
    cats:[],
    index:[]
}
```

라고 치니

'[Vue warn]: The "data" option should be a function that returns a per-instance value in component definitions.'라는 에러가 떴다.

data는 무조건 함수형으로 써주어야한다.

```
data(){ return aaa }와 data:{ return aaa} 는 다르다.
```





이렇게 적어주면 각각 파라미터에 맞게 하나씩 상세페이지가 뜬다.

완성!

