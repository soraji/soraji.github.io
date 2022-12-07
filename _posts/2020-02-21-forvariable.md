---
layout: post
title:  "[Vue.js] for문에서 변수가져오기(file1,file2,file3...file10에서 file1,file2만 가져오기)"
categories: front 
comments: true
---







참 이거는 뭐라고 설명하기가 되게 어렵다.

노출이 절대안될... 내용인데(왜냐면 설명을 못하겠음!!!)

근데 진짜 꼭 필요한 기능이단 말이다...ㅠㅠ



vue는 api에서 데이터를 받아오는데,

API에서 받아오는 key값 이름이 file1, file2, file3... file10까지 있다고 하자.

근데 사실상 내용은 file1, file2에만 들어있고, file3부터 file10까지는 그냥 공백으로 처리되어있다.

이럴때 for문을 돌려서, 값이 있는 경우에만 찍어라, 가 되어야 한다.

만약 for문을 쓰지 않는다면(말도 안되지만)

그냥 내가 div를 1개만 찍으면 실질적으로 찍어야하는 파일은 2개인데 항상 1개만 띄워지는거니 당연히 안되는거고,

그렇다고 div를 10개를 찍으면 8개는 공백으로 뜰것이다

절대 file의 개수가 몇개일지 알수없으니 for를 써야한다.

근데, 이럴때 문자열과 변수를 합쳐서 사용해야하는데... 이걸 어떻게 하느냐!!



처음에는

```
<div v-for="item in items" :key="item.id">
  <div v-for="n in 10" :key="n">
    <div>{{'item.file'+n}}</div>
  </div>
</div>
```

이런식으로 작성했는데,

암만해도 안된다 흑흑



스택오버플로우의 도움을 받아...

[자세한 내용은 여기](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors#Bracket_notation)

MDN에 보면 '속성 접근자(Bracket notation)' 라고 불리는데, `.`을 치고나면 뜨는 속성들에 대해서 알기 쉽게 정리 되어있다. 



그래서 보면,

```vue
<div v-for="item in items" :key="item.id">
  <div v-for="n in 10" :key="n">
    <div>{{ item [ 'file' + n ] }}</div>
  </div>
</div>
```

이렇게 하면 값이 있는 key값들의 이름만 받아온다.





아니 근데 왜 근데 글이 딱 그 코드만 안올라가지......;;;;;;;;;;;;;;;;;;뭐지;;;;;;;;;;;;;;

`{{ item [ 'file' + n ] }}` 이렇게 쓰면된다.





이것도 굉장히 예전에 찾아놓은거였는데, 어느정도 vue를 작업하다 보니까 모든 string과 변수의 합은 대괄호를 이용한다.

