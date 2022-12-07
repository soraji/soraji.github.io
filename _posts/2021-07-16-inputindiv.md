---
layout: post
title:  "[css] td 안의 div width 100%, div안의 input width100%"
subtitle:   ""
categories: front 
comments: true
---



우선 div를 td사이즈에 고정시켜놓으려면 td사이즈가 내용물에 영향을 받지 않도록 고정을 해주어야한다

td내용물이 많으면 width가 늘어나기 떄문에 `table-layout:fixed` 를 사용해서 td사이즈를 지정해준다.

(만약 fixed를 쓴 상태에서 colspan을 사용해야하는데 width가 내 마음대로 조절이 되지 않는다면

아래 글 참조 ( ｡ớ ₃ờ)ฅ

https://soraji.github.io/markup/2020/11/03/colgroup/ )

<br>

<br>

<br>

td를 고정해둔 상태에서 div의 width를 100%로 해놓으면 div는 웬만하면 맞을텐데 안맞는다면

`max-width:100%` `min-width:100%` 를 사용해보길 바람!



근데 input이 div를 넘어오는 경우가 있다.

input에 아무것도 주지 않았다면 맞겠지만 input박스이니만큼 안에 padding을 넣어주어야 하는 경우가 많은데

문제는 width를 잡을 때는 padding값까지 다 같이 잡아버린다는 것이다

이런 속성때문에 애 좀 먹었는데 지금에서야 발견하다니...

고정형이 아닌 반응형에서는 이게 굉장히 많이 쓰이기 때문에 정리해둔다

여튼, 

그래서 padding, margin값을 포함한 width만큼을 

해버리기때문에 100%로 암만 맞춰놓는다고 한들 border로 확인해보면 100%가 훌쩍 넘어버린다

이럴때 사용하는 것이 `box-sizing:border-box` 이다

이 css를 사용하면 padding, margin값을 제외한 width만을 잡아서 100%에 맞춰져서 나온다 :-)

고로 td안의 div안의 Input은 td사이즈에 맞춰서 반응형으로 조절할 수 있단 말씀 :-)















