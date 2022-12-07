---
layout: post
title:  "[Vue.js] #4.레거시 위에 vue.js 올리기(레거시에서 로그인 세션 vue로 가져오기)"
categories: front 
comments: true


---

*참고글*

1번. [api를 만들어 어떻게 json형식으로 데이터를 가져올것인가](https://soraji.github.io/back/2019/09/25/classASPjson/)

2번. [vue의 코드들을 어떻게 웹서버에서 올릴것인가](https://soraji.github.io/front/2019/10/31/vuewithlegacy1/)

3번. [노드서버에서(localhost:8080) 돌리는 vue를 어떻게 IIS와 연동할것인가](<https://soraji.github.io/front/2019/10/31/vuewithlegacy2/>)

4번. [레거시에서 window.open을 했을때 vue 빌드 부르기](<https://soraji.github.io/front/2019/11/02/vuewithlegacy3/>)



오늘은 마지막 글. 대망의 세션 문제이다.

## 레거시에서 로그인한 상태(세션)을 어떻게 vue로 가져올것인가



이 문제가 아무래도 가장 큰 문제였다. (해결해 보니 별거 아니었지만)

레거시에서 로그인을 하고, 세션을 유지한 유저들에게만 서비스를 제공하는데, 문제는 만약 세션 확인을 하지 못한다면 

누군가 url을 치고 들어오면 권한이 없어도(로그인을 하지 않아도) 접속을 할 수 있다는게 가장 큰 문제였다.

어떤 경로로 치고 들어오든 일정시간이 지나면 세션이 사라지기 때문에 세션값을 계속해서 확인했어야만 했다.



우선 세션값은, 서버단에서만 알수있는 값이므로 javascript로 값을 변경하거나 할수는 없다. 

ajax를 이용해서 세션값을 가져와야 한다.

나는 ajax로 세션value를 가져오는 것이아니라, 

로그인을 하면 생성되는 세션을 api로 던져 세션값이 존재하는지 안하는지를 파악했다.

지금부터 작업하는 내용은 다 vue단에서 이루어 진다.



우선 나의 vue 트리구조는 이러하다

~~~
-dist
-node_moduels
-public
-src
	-api
		api.js
	-assets(이미지)
	-components
		login.vue
		navi.vue
		main.vue
	-routes
		routes.js
	-store
		actions.js
		index.js
		mutations.js
	-views
		detail1.vue
		detail2.vue
		detail3.vue
	App.vue
	main.js
app.js
package.json
vue.config.js
~~~



---

우선, 처음에 axios로 데이터를 받아와야한다.

~~~js
//api.js

import axios from 'axios';

const config = {
  baseUrl: 'API URL'
}

function fetchSession(){
  return axios.get(`${config.baseUrl}getSession.asp`);
}

export{
  fetchSession
}
~~~

그러면 이 fetchSession을 가져가서 사용할 수 있는 상태가 된다.

나는 navi.vue에 접속되었을때 created()로 세션을 받아온뒤 

세션이 있으면 main컴포넌트를 보여주고, 

세션이 없으면 로그인 컴포넌트로 보낼것이다.

~~~vue
//navi.vue
<template>
  <div>
    <div style="display:none;">{{this.$store.state.session}}</div>
  </div>
</template>

export default {
  created(){
    this.$store.dispatch('FETCH_SESSION');	//vuex를 통한 상태관리를 해준다.FETCH_SESSION함수를 시작
  },
  methods:{
    session: function(gubun) {
      console.log('gubun'+gubun);
      this.$emit('getSession',gubun);	
    }
  },
  updated(){
    console.log('updated'+this.$store.state.session);
    this.session(this.$store.state.session);
  }
}
~~~

돔에 렌더링하기 전에 FETCH_SESSION함수를 부르고 실행한다.

methods에서는 session이라는 함수를 만들어 getSession이라는 함수에 gubun이라는 파라미터를 얹어 부모컴포넌트(App.vue)로 보낸다

※ emit으로 부모에게 이벤트를 올릴떄 파라미터를 같이 올리고 싶으면 함수명 다음에 컴마(,)를 찍은뒤 변수를 보내준다.

그런데 문제는 store/index.js안에 있는 FETCH_SESSION함수를 실행하게 되면 FETCH_SESSION에 있는 state값만 가져온다.

왜냐면 두번 로드하지 않기 때문에 초기값만 가져오기 때문이다. 그래서 dom에 다 찍은 이후 다시 업데이트 해주기를 원하면 updated로 session메소드를 다시 불러주어야한다.

~~~js
//store/index.js

import Vue from 'vue';
import Vuex from 'vuex';
import {fetchSession} from '../api/api.js';

Vue.use(Vuex);

export const store = new Vuex.Store({
  state:{
    session:1
  },
  mutations:{
    SET_SESSION(state, data){
      state.session = data;
    }
  },
  actions:{
    FETCH_SESSION(context){
      fetchSession()
      .then(response =>{
        console.log('fetchSession'+ response.data.session[0].gubun);
        if(response.data.session[0].gubun == 1){  //세션없으면:0 / 있으면:1
          context.commit('SET_SESSION',false);
        }else{
          context.commit('SET_SESSION',true);
        }
        
      })
      .catch(err=>{
        console.log(err);
      })
    }
  }
})

~~~



그러니까, 세션이 있다고 가정하고 navi.vue에서 created를 하게 되면 session은 state에서 지정해준대로 1을 계속해서 반환한다.

하지만 세션이 있으면(1) true로, 없으면(0) false값으로 다시 받아오고 싶었기에

navi.vue에서 updated()로 다시 불러온다. 그러면 true/false를 리턴한다.



결국 `display:none`이라 찍히지는 않지만 돔 어딘가에 `this.$store.state.session` 은 true혹은 false로 찍히게 된다.

true로 세션이 있으면 main.vue 컴포넌트를 띄워주면 되고, false로 세션이 없으면 login컴포넌트를 띄워준다.



현재 컴포넌트 구조를 보면 App.vue(상위컴포넌트)아래 navi, main, login컴포넌트가 하위 컴포넌트로 등록이 되어있는거라

navi에서 받아온 true/false값을 가지고 컴포넌트 display를 결정한다.

~~~vue
//App.vue

<template>
  <div id="app">
    <Navi v-on:getSession="defSession"></Navi>
    <div v-if="Session == true">
      <Main></Main>
    </div>
    <div v-else>
      <login></login>
    </div>
  </div>
</template>

<script>

import Navi from './components/navi.vue';
import Main from './components/main.vue';
import login from './components/login.vue';
export default {
  components:{
    Navi,
    Main
    login
  },
  data(){
    return{
      Session:false,
    }
  },
  methods:{
    defSession: function (gubun) {
      console.log('gubun'+gubun);
      if(gubun == true){
        this.Session = true;
      }else{
        this.Session = false;
      }
    }
  }
}
</script>
~~~



navi는 항상 뜨고 session을 확인하는 용도로 적고 거기서 받아온 값으로 컴포넌트를 `v-if`로 바인딩 해준다.

`data`로 session의 기본값을 정해놓고 아까 emit으로 올린 `v-on`바인딩된 getSession 이벤트명을 받아 defSession 메소드를 실행시킨다.

defSession 메소드를 아까 emit올릴때 보낸 parameter와 같이 받아 gubun이 true면 data의 Session은 true고, 

gubun이 false면 data의 Session은 false다.

그 이후로  v-if로 `Session == true` 이면 main컴포넌트를 받아오고 아닐때는 login컴포넌트를 받아온다.



세션에 따라 로그인창으로 보내거나 메인창으로 보내는거 성공 =)