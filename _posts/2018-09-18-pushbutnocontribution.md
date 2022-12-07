---
layout: post
title:  "깃허브 푸시는 되는데 컨트리뷰션 그래프에 찍히지 않을때"
subtitle:   "etc"
categories: error
comments: true


---



일할때 쓰는 윈도우 데스크탑 하나, 개인 맥북프로 하나를 가지고 있어서 총 2개의 컴퓨터를 이용하고 있는데, 환경은 거의 유사하게 둘다 세팅이 되어있다.

특히 배운걸 이쪽저쪽 왔다갔다 사용하기 때문에 두개 다 깃헙, 소스트리를 사용하는데 소스트리말고 터미널에서 깃허브를 조작하고싶다는 욕심이 생겨서, 윈도우 터미널에서 깃헙을 조작하다가,

어느순간부터는 윈도우에서는 푸시는 되는데 컨트리뷰션 그래프에 찍히지 않는 현상이 일어나기 시작했다 ㅠ.. 1일1커밋을 목표로 하고있는 상황에서 커밋을 하는데도 컨트리뷰션에 찍히지 않으니 작업은 윈도우에서 훨씬 많이 하는데 너무너무 답답했다 ㅠㅠ

아니 푸시는되는데 왜 초록색 잔디밭 안생기냐고....!

그래서 stackoverflow에서 찾아봤더니,

이메일의 문제였다.



터미널에서

~~~
git config --global user.email
~~~

를 치면, 사용자 이메일이 뜨는데, 이메일이 맞는지 먼저 확인하고

~~~
git config --global user.email
	#=>사용자이메일
git config --system user.email
	#=>
git config --local user.email
	#=>사용자이메일
~~~

git 명령어를 차례로 써보면 --system은 안나와도 되고, --local은 나와야하는데, 나같은 경우는 local user.email이 등록되어있지 않아서 컨트리뷰션에 찍히지 않은것이었다!!

그래서 

~~~
git config --local user.email wlthfk0211@gmail.com
~~~

이라고 등록해주고, 다시 

~~~
git config --local user.email
~~~

이라고 명령을 하니 wlthfk0211@gmail.com 이라고 사용자 메일이 잘 떴고, 깃헙 overview로 들어가보니 컨트리뷰션 그래프에도 잘 찍혀있었다 =)





![contributions](/assets/img/contributions.png)



크 정말 이런 그래프.. 존경한다 ㅠㅠ


