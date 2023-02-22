---
layout: post
title: "[ Java ] 셀레니움으로 알럿 확인(selenium alert confirm)"
subtitle: ""
categories: back
comments: true
---

만약 headless 셀레니움으로 크롤링을 돌리는데 뭐 예를들어 '맞는 정보가 없습니다' 따위의 알림이 계속해서 뜨면

알림을 없애주어야한다. 안그러면 다음으로 진행이 안되니까...

만약 한 필드에 있는 값을 for문으로 돌려서 셀레니움으로 돌려야하는데 5개는 정보가 없다며 알림이 뜨고, 5개는 정보가 있다면

알림이 있는지 없는지 확인한 후

알림이 있다면 확인을 누르고,

알림이 없다면 source를 가져오는 알고리즘을 짜야한다.

우선, 알림창이 존재하는지 안하는지 먼저 확인을 하려면

```java
if(ExpectedConditions.alertIsPresent().apply(driver)==null){

			}
```

위 코드를 사용한다. 위 코드는 알림창이 없을때의 if문이고, else로는 알림창이 존재했을때 알림창 확인을 눌러 OK를 해준다.

```java
if(ExpectedConditions.alertIsPresent().apply(driver)==null){
	//알림창이 없으면 아무것도 하지 말것
	thtml = driver.getPageSource();
	debugnew(2, "result97","G2bCrawlingResult002","thtml : "+thtml , G2bCrawlingSmallGui.getOutput("result97"));
}else{
      //알림창이 존재하면 알림창 확인을 누를것
      driver.switchTo().alert().accept();
}
```
