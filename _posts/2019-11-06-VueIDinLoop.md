---
layout: post
title:  "[Vue.js] Vue for문에서 id값주기(vue id-binding in loop)"
categories: front 
comments: true


---





vue를 하다가 for문에서 id를 주어야하는 상황이 생겼다.

아니 다들 말을 너무 어렵게 한다...내가 못따라 가는건가.



보통은 for문에서 돌려서 리스트를 뽑아오는데, 예전에는 asp로 

```asp
<input type="checkbox" name="check<%=i%>" id="check<%=i%>">
<label for="check<%=i%>"><span></span></label>
```

과 약간의 css로 커스터마이징을 했었다.

저렇게 하면 DOM에는 check0, check1, check2, check3...이런식으로 고유의 아이디값이 생기는거라서 

체크박스를 체크할 수 있었는데



vue로 하니 

아니 이게 도대체 어떻게 써야하는거야.....? 처음엔 아예 알아듣지못했다..ㅠㅠ

에러에 근데 id="{{ val }}" 이런식으로 쓰지 말라고!! 적혀있었다

처음에는 id="check{{i+1}}" 이런식으로 적었는데 에러 계속뜸...!

알고봤더니 

```
v-bind:id="'문자열'+변수"
```

이렇게 해야지 돔에서도 '문자열변수' 로 각각의 아이디값으로 렌더링됨

```vue
<input type="checkbox" v-bind:name="'check'+i" v-bind:id="'check'+i">
<label v-bind:for="'check'+i"><span></span></label>
```

css속성은 똑같이 이렇게 v-bind해줬더니 체크박스 커스터마이징 완료 =)