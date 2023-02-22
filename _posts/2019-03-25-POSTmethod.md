---
layout: post
title: "[ Java ] java에서 POST 방식으로 response가져오기2"
categories: back
comments: true
---

```
//파라미터를 제외한 타겟url
URL url = new URL("타겟url");

HttpURLConnection hConn = (HttpURLConnection)url.openConnection();
hConn.setDoOutput(true);
hConn.setRequestMethod("POST");
PrintWriter pw = null;

//파라미터를 붙인다
pw = new PrintWriter(new OutputStreamWriter(hConn.getOutputStream(), "utf-8"));
pw.print("pageIndex=1");
pw.print("&searchSidoho=");
pw.print("&searchKsangho="+searchcompany);
pw.print("&searchSingoYear=2018");
pw.print("&searchUpjong=27");
pw.print("&searchStrPrice=");
pw.print("&searchEndPrice=");
pw.print("&searchOrder=1");
pw.print("&jsonView=N");

pw.flush();
pw.close();
hConn.connect();

StringBuffer resultb = new StringBuffer();
BufferedReader in = null;

String inputLine;
in = new BufferedReader(new InputStreamReader(hConn.getInputStream(),"utf-8"));
while ((inputLine = in.readLine()) != null) {
resultb.append(inputLine);
resultb.append("\n");
}
in.close();

String result = resultb.toString();
```

POST 방식은 GET 방식과 달리, 데이터 전송을 기반으로 한 요청 메서드이다.

GET방식은 URL에 데이터를 붙여서 보내는 반면, POST방식은 URL에 붙여서 보내지 않고 BODY에다가 데이터를 넣어서 보낸다. 따라서, 헤더필드중 BODY의 데이터를 설명하는 Content-Type이라는 헤더 필드가 들어가고 어떤 데이터 타입인지 명시한다.

컨텐츠 타입으로는 여러가지가 있지만, 몇가지를 적자면,

1. **application/x-www-form-urlencoded**
2. **text/plain**
3. **multipart/form-data**

등이 있다.

따라서 POST 방식으로 데이터를 보낼때는 위와 같이 컨텐츠 타입을 꼭 명시해줘야한다.

보통 작성하지 않는 경우는 1번의 컨텐츠 타입으로 셋팅된다.

1번의 컨텐츠 타입은, GET방식과 마찬가지로 BODY에 key 와 value 쌍으로 데이터를 넣는다. 똑같이 구분자 &를 쓴다.

2번의 컨텐츠 타입은, BODY에 단순 txt를 넣는다.

3번의 컨텐츠 타입은, 파일전송을 할때 많이 쓰는데 BODY의 데이터를 바이너리 데이터로 넣는다는걸 알려준다.

자바와 같이 oop 프로그래밍에서는 BODY에 데이터를 InputStream/OutputStream 클래스를 통해서 읽고/쓰고 한다.

출처:

https://mommoo.tistory.com/60

[개발자로 홀로 서기]

---
