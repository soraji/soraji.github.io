---
layout: post
title: "[ Java ] jsoup Parsing Read timed out error jsoup파싱에러"
categories: error
comments: true
---

```java
String url = "가져오고자하는 url";
String selector = "DOM에서 css selector경로 copy(feat.개발자도구)";
Document doc = null;

try {
    doc = Jsoup.connect(url).timeout(30000).get();
} catch (IOException e) {
    System.out.println(e.getMessage());
}

String TheDate = doc.select(selector).text();
```

처음에 jsoup으로 파싱을 하려는데 계속 Read timed out에러가 뜨는거다.

알아봤더니 각 사이트에서 대기시간같이 잠깐의 시간을 주어야지 접속이 가능한 사이트들이 있어서

중간에 timeout을 지정해주면 된다.

그리고 try/catch로 묶는다.

doc에다 html을 담은 뒤 원하는 엘리먼트를 선택한다.

처음에는 TheDate를 String이 아닌 Element로 넣었는데 그러고보니 String과 비교를 할 수가 없었다.

그래서 Element를 String으로 바꾸고 셀렉터뒤에 text만 추출하도록 .text()를 붙여줌

Element를 String으로 바꾸려면 transform api가 있어야한다는 등 뭐 그런말들이 있었는데

난 그렇게까지는 필요하지 않아 그냥 text로 추출해서 String으로 바꿔버렸다.

그나저나 이런식으로 엘리먼트추출하고 슬랙이랑 같이 쓰니 요물이다 요물..
