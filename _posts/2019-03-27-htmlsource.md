---
layout: post
title: "[ Java ] java에서 html소스 불러오기"
categories: back
comments: true
---

java에서 HTML 소스 불러오기

```java
String urlPath = "http://www.example.com";
String pageContents = "";
StringBuilder contents = new StringBuilder();

try{

    URL url = new URL(urlPath);
    URLConnection con = (URLConnection)url.openConnection();
    InputStreamReader reader = new InputStreamReader (con.getInputStream(), "utf-8");

    BufferedReader buff = new BufferedReader(reader);

    while((pageContents = buff.readLine())!=null){
        //System.out.println(pageContents);
        contents.append(pageContents);
        contents.append("\r\n");
    }

    buff.close();

    System.out.println(contents.toString());

}catch(Exception e){
    e.printStackTrace();
}
```
