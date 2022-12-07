---
layout: post
title:  "[Vue.js] Vue를 적용하니 IE11에서 하얀화면(blank)으로 나올때"
categories: front 
comments: true

---



혹시나 다른이유때문에 안나오는것같으면 뒤의 2,3편도 참고해주세요

[Vue를 적용하니 IE11에서 하얀화면(blank)으로 나올때 2편](https://soraji.github.io/vue/2020/06/25/vueIEBlank2/)

[Vue를 적용하니 IE11에서 하얀화면(blank)으로 나올때 3편](https://soraji.github.io/vue/2020/08/07/IEblank3/)







vue로 실컷 작업한뒤, chrome에서 신나게 작업을 하고 있는데,

갑자기 들려오는 말 "소라씨 이거 IE에서는 아예 안나오는데? blank가 떠"

..............

IE는 악의 근원임에 틀림없다.

브라우저 엔진은 어느세월에 업데이트 할건지.................

3시간동안 삽질을 한 결과,



IE는 ES6를 읽을 수 없어 생긴 문제라고 한다.

근데, 저번에 내가 IE에서 띄웠을때는 잘 나왔었는데, 왜 이번거만 안되는지 모르겠다.

뭐.. 이것저것 쓴 라이브러리들이 많아서 그런가..



정확한 원인은 잘 모르겠다.

하지만, ES6를 IE에서 지원해주지 않아 

vue.config.js와 babel을 설정해주어야 한다는 것, 

polyfill을 사용해야한다는 것을 알아냈다.



바벨7과 바벨6의 버전차이로 인해 config파일을 인식하지 못할수도 있다고 하고,

store : store, router: router를 store, router, 로 써서 못알아듣는다고 하기도 하고..

이 외에도 너무 많은 이유들이 있다고...



그치만 가장 답답한건 IE의 개발자도구 는 정말 형!편! 없다는 거다.

정말 형편없어도 이렇게 없을까...너무너무 불편하다.

크롬안보나???????????? 크롬 반만되도 좋겠네



너무 많은 npm을 설치해서 어떤것때문에 작동하는지는 잘 모르겠지만(이것저것 너무 많이 해봄 흑흑)

```
npm install --save @babel/polyfill
```

으로 바벨을 설치해준다.



```
//package.json
"devDependencies": {
    "@babel/cli": "^7.7.4",
    "@babel/core": "^7.7.4",
    "@babel/preset-env": "^7.7.4",
    "@vue/cli-plugin-babel": "^4.0.0",
    "@vue/cli-plugin-eslint": "^4.0.0",
    "@vue/cli-service": "^4.0.0",
    "babel-cli": "^6.26.0",
    "babel-core": "^6.26.3",
    "babel-eslint": "^10.0.3",
    "babel-loader": "^8.0.6",
    "babel-preset-env": "^1.7.0",
    "eslint": "^5.16.0",
    "eslint-plugin-vue": "^5.0.0",
    "vue-template-compiler": "^2.6.10"
  },
```



설치가 완료되면 /src/main.js에

```
import '@babel/polyfill'
import Vue from "vue";
```

을 작성한다. 반드시 Vue 위에 써주어야한다. 그러니까 최상단에 써주어야 함.

그리고 루트의 babel.config.js에

```
module.exports = {
  presets: [
    [
      '@vue/app',
      {
        "useBuiltIns": "entry"
      }
    ]
  ]
}
```

를 작성한다.



vue cli3 으로 프로젝트로 만들때 디폴트로 만들게 되면 따로 .babelrc 파일이 생성되지 않는다.

그런데 난 필요하니까 .babelrc 라는 이름의 파일을 새로 생성해주자..

그리고 그 파일 안에다는

```
{
  "presets": ["@babel/preset-env"]
}
```

을 넣어준다.



vue.config.js에는

---

*2021-07-05 내용추가합니다*

*아래코드중 transpileDependencies: [ansiRegex] 이라고 되어있는 부분은 node_modules에 혹시 수정한게 있는 분들만 적어주시고, 아니라면 주석처리해주세요*

*기효님이랑 얘기하다가 알게됐는데 제 코드를 그냥 복사해서 가져가셨다가 규모가 엄청 커진경우가 있다고 하셔서ㅋㅋㅋㅋㅋ*

*저는 node_modules에서 수정해야할 코드가 있어서 직접 수정을 했는데,*

*transpileDependencies: [ansiRegex] 이 코드는 node_modules에 있는 모든 라이브러리들을 컴파일하라는 명령이기 떄문에 node_modules에 딱히 수정한게 없으신분들은 이 문장을 빼고 컴파일하셔야 용량이 줄어들거에요!*



```javascript
const ansiRegex = require('ansi-regex')
module.exports = {
  lintOnSave : false,
  publicPath: 'dist/',
  assetsDir: process.env.BASE_URL,
  //아래 코드는 node_modules에 수정사항이 있으면 살려두고 아니면 주석처리
  transpileDependencies: [ansiRegex]
}
```

이렇게 작성해주었더니 IE에서 내가 만든 코드들이 나왔다 엉엉

진짜 엄청 기쁨과 동시에 IE의 사악함에 짜증이 났다.

매번 이런식으로 IE에서 지원한되는걸 해야한다니...... 눈앞이 깜깜하다.......

더도말고 덜도말고 크롬과만 같아라.....









정말 많은 글을 보았다.

can't get Vue working in IE 11.. blank ie 11 vue... 정말 20개 넘는 글을 보고 다 해봤는데도 안됐는데

기본기가 부족하니 문제해결만 하기 급급해서 작동원리가 정확하게 뭔지를 모르겠다.

빌드과정과 크로스브라우저에 대해서 잘 인지하려면 바벨과 웹팩을 공부해야할것 같다.







