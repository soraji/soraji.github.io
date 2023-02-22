---
layout: post
title: "[ Java ] 자바selenium(셀레니움)으로 엘리먼트 클릭하고 현재url 알아내기"
categories: back
comments: true
---

```java
driver = new ChromeDriver(options);
driver.get("원하는 url");
```

셀레니움으로 브라우저를 띄우고 원하는 url로 이동한다

```java
WebElement el = driver.findElement(By.cssSelector("#pager > button.btn-last"));
el.click();
```

개발자도구에서 원하는 엘리먼트를 선택한 다음 마우스 오른쪽버튼을 눌러 copy에서 cssSelector를 선택한다.

그러면 css셀렉터기준으로 엘리먼트의 고유값을 얻을 수 있다.

그리고는 `driver.findElement(By.cssSelector("셀렉터패스"))` 를 넣으면 원하는 엘리먼트들이 선택된다.

클릭을 원하면 그 엘리먼트 뒤에 `.click()`을 해주면 된다.

```java
String url = driver.getCurrentUrl();
```

현재url의 주소를 알고싶다면 `driver.getCurrentUrl`로 알아낼 수 있다.
