---
layout: post
title:  "[Vue.js] vue에서 Font Awesome 사용하기"
categories: front 
comments: true


---



```
$ npm i --save @fortawesome/fontawesome-svg-core
$ npm i --save @fortawesome/free-solid-svg-icons
$ npm i --save @fortawesome/vue-fontawesome
```



로 fontawesome을 설치한다.(브랜드아이콘, pro아이콘 등등은 저 외에 다른 npm이 있다)



[공식docu](<https://github.com/FortAwesome/vue-fontawesome>)를 보면 자세하게 나와있긴한데, 좀 이것저것 경우의 수가 많아서 정리한다.

처음에 라이브러리를 다운받고나면

main.js에

```javascript
//main.js

import Vue from 'vue'
import App from './App.vue'

import { library } from '@fortawesome/fontawesome-svg-core'
import { fas } from '@fortawesome/free-solid-svg-icons'
import { far } from '@fortawesome/free-regular-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(fas)
library.add(far)

Vue.component('font-awesome-icon', FontAwesomeIcon)

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
```



pro를 구매하려면 뭔가 다른게 있어야한다고 한다.

하지만 난 pro가 아닌 solid혹은 regular만 사용할테니 저렇게만 다운받고 add시켜주었다.



그리고

app.vue에 

```vue
//app.vue

<template>
  <div id="app">
    <font-awesome-icon icon="user-secret" />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>
```

로 사용하게 되면 바로 user-secret 에 맞는 아이콘이 보여지게 된다.



하지만 난 app.vue에 쓰고싶은게 아니라 다른 컴포넌트에서 사용하고 싶은거다!!

이럴때는 우선 app.vue에서 사용하고 싶은 컴포넌트 등록을 해주고

예를들어 header.vue를 등록한다 하면,

```vue
//header.vue

<template>
  <div>
    <ul>
      <li><font-awesome-icon :icon="faQuestionCircle" />｜</li>
      <li><font-awesome-icon :icon="faMapMarkerAlt" />｜</li>
      <li><font-awesome-icon :icon="faUser" />｜</li>
      <li><font-awesome-icon :icon="faHeart" />｜</li>
      <li>({{ itemcount }}) <font-awesome-icon :icon="faShoppingBag" /></li>
    </ul>
  </div>
</template>

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faMapMarkerAlt } from '@fortawesome/free-solid-svg-icons'
import { faQuestionCircle } from '@fortawesome/free-regular-svg-icons'
import { faUser } from '@fortawesome/free-regular-svg-icons'
import { faHeart } from '@fortawesome/free-regular-svg-icons'
import { faShoppingBag } from '@fortawesome/free-solid-svg-icons'

export default {
  components: {
    FontAwesomeIcon
  },
  data () {
    return {
      itemcount : 0,
      faMapMarkerAlt,
      faQuestionCircle,
      faUser,
      faHeart,
      faShoppingBag
    }
  },
}
</script>
```



이렇게 사용하면 된다.

import를 해줄때 fa뒤에 대문자로 원하는 아이콘의 첫 영문자를 쓰면 자동으로 로딩이 된다.(-는 사용할수없다)

data로 선언해주고 그걸 갖다 쓴다.









