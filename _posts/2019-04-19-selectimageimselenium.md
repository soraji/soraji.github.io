---
layout: post
title: "[ Java ] 이미지 선택자 in selenium"
categories: back
comments: true
---

```java
WebElement temp = driver.findElement(By.xpath("//img[@src='web/L001/images/IMAGENAME.jpg']"));
```

브라우저의 내용을 단순히 가져오는것이 아닌 브라우저를 컨트롤하려는 것이 셀레니움을 사용하는 가장 큰 이유이다.

그중에 html코드를 개떡으로 만들어놓은 사이트들이 있어서(예를들어 인천공항이라던가... 혹은 인천공항이라던가.. 아님 인천공항이라던가..)

id도 없고, class도 없고, name도 없고... img를 선택하는게 훨씬 나을때,(로그인 이미지는 웬만하면 1개이니)

그럴때 저 이미지를 찾는 xpath를 사용한다.

webElement를 사용하면 text를 보낼수도 있고, 클릭을 할수도 있고, 내용을 지울수도 있다.
