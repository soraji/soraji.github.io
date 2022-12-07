---
layout: post
title:  "토르 연결실패 했을때(TOR failed to establish a TOR network connection)"
subtitle:   ""
categories: error
comments: true


---

TOR를 설치하고 처음으로 connect를 시키려고 버튼을 눌렀는데 tor failed to establish a tor network connection(clock ----)이라는 오류가 뜨길래 뭔가 하고 찾아봤더니 토르는 정확한 시간을 설정하는게 매우 중요한가보다.

그래서 시간대를 정확하게 설정해주는게 중요하다.

우선, 시간을 설정하는 '날짜 및 시간'에 들어가서 인터넷시간을 들어가보자. 동기화가 되는지 안되는지 확인한다.

만약 동기화를 눌렀는데 실행할 수 없다고 뜬다면 컴퓨터검색에서 services.msc를 누른뒤 window time을 찾아서 자동인지 수동인지를 확인해주어야한다. 

자동이 아니라면 자동을 선택한뒤 적용- 시작을 누르면 시간이 인터넷에 있는 시간과 동기화가 되는데, 그 이후로 시간은 잘 맞는거라고 생각하면 됨.

하지만 내 문제는 오후 12시를 오전12시로 해놔서 그런거였음... ㅂㄷㅂㄷ

시간 맞추니까 잘컨넥됨.

