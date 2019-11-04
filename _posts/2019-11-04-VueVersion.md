---
layout: post
title:  "[Vue.js] Vue Version 비교 (cli2 vs cli3)"
categories: front 
comments: true
---


# vue cli 버전비교

## CLI 2.x VS CLI 3.x

cli 3을 사용하려면 

```vue
npm install -g @vue/cli
```

설치해주어야한다.

만약 다시 2로 돌아가려면 

```
npm install -g vue/cli
```

로 다시 설치해 주어야함





-명령어

- 2 : vue init '프로젝트 템플릿 이름' '파일위치'
- 3 : vue create '프로젝트 이름'

-웹팩 설정 파일

- 2 : 노출O
- 3 : 노출 X

-프로젝트 구성

- 2: 깃헙의 템플릿 다운로드 
  https://github.com/vuejs-templates/webpack-simple/tree/master/template 에 있는 템플릿을 다운받음
- 3 : 플러그인 기반으로 기능추가
  원하는 플러그인만 설치가능. 프로젝트 중간에도 추가할 수 있다.

-ES6 이해도

- 2 : 필요 X
- 3 : 필요 O

-node modules

- 2 : 자동설치 안됨. npm i 필요
- 3 : 자동설치

