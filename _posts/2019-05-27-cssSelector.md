---
layout: post
title:  "CSS Selector에서의 공백(space)"
subtitle:   ""
categories: web
comments: true

---

크롤링을 하다보면 css셀렉터가 아주 유용하게 쓰인다. 이때, 클래스앞에는 . ,아이디앞에는 #을 붙이는데 그 사이에 space가 들어가있다면?

예를들어 .class1.class2와 .class1 .class2는 다르다. 중간에 space를 가지고있기 때문이다.



스페이스는 **하위** 엘리먼트에서 모두 조건에 맞는 엘리먼트를 찾는것

붙어있으면 **같은** 엘리먼트에서 모든 조건에 맞는 엘리먼트를 찾는것

~~~html
<div class="ah_roll_area">
	<ul class="ah_i">
		<li class="ah_item no1">
			<span class="ah_k">윤지호</span>
		</li>
		<li class="ah_item no2">
			<span class="ah_k">차재이</span>
		</li>
		<li class="ah_item no3">
			<span class="ah_k">장자연</span>
		</li>
	</ul>
</div>
~~~

만약 위와같은 html코드가 있을때 차재이 를 크롤링하고 싶으면 css selector을 어떻게 선택해야할까?



.ah_item .no2>.ah_k

=>ah_item의 하위 엘리먼트중에서는 no2라는 클래스를 가진 엘리먼트가 없다. ah_item의 하위 엘리먼트들의 이름은 전부 ah_k이다.

.ah_item.no2>.ah_k

=>ah_item이 3개가 있는 상황에서 같은 엘리먼트에 no2을 클래스로 갖는 엘리먼트는 ah_item no2를 클래스로 갖는 li뿐이다. 그 li의 하위클래스가 ah_k이니 ''차재이''를 선택하려면 .ah_item.no2>.ah_k를 선택해야한다.



~~~
<div class="test1 test2">
	<div class="test3">
		<p class="test4"></p>
	</div>
</div>
~~~

test1 .test4는 <p class="test4">가 선택되는 것이다

test1.test2는 <div class="test1 test2">가 선택되는거고 .

