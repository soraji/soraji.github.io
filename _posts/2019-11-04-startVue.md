---
layout: post
title:  "[Vue.js] Vue를 시작해보자"
categories: front 
comments: true
---


# npm 으로 vue 시작하기

```
node -v
npm -v
```

으로 노드와 npm이 잘 설치되었는지 확인하고, 

터미널에서

```
npm i vue
```

```
npm i vue-cli
```

로 vue를 설치해준다.



그리고 진행할 프로젝트를 생성한다.

vue cli3을 연습해보고싶었으므로(2도 제대로 못하면서...)

폴더를 생성하고 cd 한뒤 

```
vue create vue-project
```

를 실행하니

```
Vue CLI v3.11.0
? Please pick a preset: (Use arrow keys)
> default (babel, eslint)
  Manually select features
```

라면서 물어본다. vue cli 3인것을 확인할수있다



디폴트를 선택해서 프로젝트를 생성하면 프로젝트명으로 된 폴더가 생겨난다.

그 뒤로

```
cd vue-project
```

로 디렉토리 옮겨주고 거기서

```
npm run serve
```

해주면 로컬이 실행된다.