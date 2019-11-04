---
layout: post
title:  "[Vue.js] Vue Router(뷰 라우터)에 대해 알아보자"
categories: front 
comments: true
---


라우터는 route가 '길'이라는 뜻을 가진만큼 방향에 대한 의미를 내포하고 있는데 

기존 사이트들이 파라미터로 경로를 설정해주었다면 이제는 router가 그 역할을 한다고 생각하면 된다.

그래서 뒤에 확장자 명이 붙지않고 대게 단어로 끝나는 경우가 많으며 path를 지정해줄수도 있다.


src폴더안에 views라는 폴더를 만든뒤

url로 가면 뿌려질 컴포넌트파일을 만든다.(페이지라고 생각하면 된다.)

url과 url을 입력하고 들어가면 처음으로 뿌려지는 화면이 세트이듯이 router의 index.js에는 path와 component가 짝으로 들어있다.



화면에 뿌려지는 컴포넌트(페이지)는 views라는 폴더에 vue확장자로 파일을 만들고, router폴더의 index.js에서 import해준다.

import할때 { } 넣지 말기.... 잘못보고 넣었다가 내용이 안나온다..

<router-view></router-view>했는데 내용이 안나와서 한참 애먹음



화면단의 vue파일들에는 스캐폴드로 템플릿을 만들면 되는데 예전에는 scf였는데 지금은 그냥 vue치니까 템플릿 자동완성이 된다!

```vue
//index.js

import Vue from 'vue';
import VueRouter from 'vue-router';
import NewsView from '../views/NewsView.vue';
import AskView from '../views/AskView.vue';
import JobsView from '../views/JobsView.vue';


Vue.use(VueRouter);

export const router = new VueRouter({
  routes : [
    {
      path:'/news',
      component:NewsView,
    },
    {
      path:'/ask',
      component:AskView,
    },
    {
      path:'/jobs',
      component:JobsView,
    }
  ]
});
```
