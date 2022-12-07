---
layout: post
title:  "[css] bootstrap-select에서 background-color바꾸기"
subtitle:   ""
categories: front 
comments: true
---



현재 작업하고 있는프로젝트에 bootstrap-select를 사용하고 있는데

이게 분명히 예전에 인라인으로 스타일을 적용했을때에는 먹었던것 같은데

왜 지금 다시 하니까 안먹는거지?????

예를 들어

~~~html
<select class="selectpicker selectDropDown show-tick" name="foo" id="foo" v-model="foo" style="background-color:#ff0000">
	<option value="0">1번</option>
	<option value="1">2번</option>
	<option value="2">3번</option>
</select>
~~~



이렇게 했을때 스타일이 먹었던것 같은데 왜 안먹냐고오오오오

dom에 뜬거 분석해보니까 select가 dom에서 렌더링될때는 button으로 바뀌어서 스타일이 안먹는건데

분명히 예전에는 잘됐던것 같은데 왜 그를까 얘가 증맬루...



결국은 다른 방법을 찾아야했듬

보통은 css에 

~~~css
select[name="foo"]{color:#ff0000}
~~~

이런식으로 스타일을 주면 foo라는 이름을 가진select에만 그 스타일이 먹는데,

bootstrap에서는 

```css
button[data-id="foo"]{background-color: #fff0f0 !important;}
```

이런식으로 줘야 먹는다

dom에 렌더링된 기반으로 스타일을 줘야 먹더라..



뭐 그렇게 오래 삽질하지는 않았지만 예전에 됐던게 안되는것 같아서 혹시나 또 까먹을까봐 기록....(끄적끄적)

