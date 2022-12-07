---
layout: post
title:  "[css] content속성 크롬은 나오는데 IE에서는 안나온다?"
categories: front 
comments: true


---



css에서 내용을 넣을때

이미지를 넣으려면 원래 `backgroud-image`를 쓰는데 부트스트랩에서는 먹지 않는다.

뭔가 다른게 같이 걸려있나... 

그래서 `content`를 사용했는데, 다행이 이미지는 뜨는데 크롬만 뜨고 IE는 뜨지 않는다.

이 content라는게 가상요소가 필요하다고 한다. 

가상요소는 Pseudo-Element 라고 하는데, 존재하지 않는 클래스를 마치 존재하는 것처럼 선택을 가능하게 만들어준다.

제일 많이  보는게 `::before` 과 `::after`인데, (fontawesome 할때 많이 봄)



이 content가 IE에서 읽을때는 반드시 가상요소가 필요하다고 한다.

크롬은 규칙이 느슨해서 알아서 렌더링해주는거라 잘나오지만 IE는 빡빡하니 내가 설정해주어야했다.



```css
/* 크롬 */
.bootstrap-select.btn-group.show-tick .dropdown-menu li.selected a span.check-mark
 {position:absolute;display:inline-block;right:15px;margin-top:0px;
  content: url( "../assets/smcheckgreen.png" );  
}

/* IE :contetn가 나오려면 after가 있어야함 */
.bootstrap-select.btn-group.show-tick .dropdown-menu li.selected a span.check-mark:after
{display:inline-block;right:20px;margin-top:0px;
 content: url( "../assets/smcheckgreen.png" );
}
```

아예 먹는것도 크롬이랑 IE랑 달라서 스타일 다르게 줘도 다르게 먹는다

신기한 브라우저의 세계...

