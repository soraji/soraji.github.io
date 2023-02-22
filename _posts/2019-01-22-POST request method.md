---
layout: post
title: "[ Java ] java에서 POST 방식으로 response가져오기"
categories: back
comments: true
---

리스트 소스는 불러오는데 상세페이지를 불러오지 않음

post방식으로 불러서 그런건데 post방식으로 부르면 파라미터랑 쿠키를 보내주는 방식으로 보내주면 됨

```
//페이지에서 response를 못받아옴.
//request값을 다 던져주면 response는 올수밖에없음. 그래서 쿠키값을 넘겨줌.
//getUrlStrcookiechungnam은 배열이기 때문에 [0]에 담음.
//변수1은 리스트목록, 변수2는 상세페이지, 변수3은 엔코딩
orghtml = UrlText.getUrlStrcookiechungnam("타겟 URL",weburl,"UTF-8")[0];
```

```

hConn.setDoOutput(true);
//POST로 받음
hConn.setRequestMethod("POST");

PrintWriter pw = null;
if(tcode.equals("")) {
    pw = new PrintWriter(new OutputStreamWriter(hConn.getOutputStream(), "KSC5601"));
} else {
    pw = new PrintWriter(new OutputStreamWriter(hConn.getOutputStream(), tcode));
}
//post로 받으면 전체 url을 쓰는게 아니고 뒤에 파라미터들만 보내는데
//개발자도구의 source를 print("")안에 넣으면 POST로 던져줌
pw.print("mnu_cd=CNNMENU00138&board_seq=179712&code=29&menuTitle=%EC%B6%A9%EC%B2%AD%EB%82%A8%EB%8F%84+%EC%9D%BC%EB%B0%98%EC%9A%A9%EC%97%AD+%EC%A0%81%EA%B2%A9%EC%8B%AC%EC%82%AC+%EC%84%B8%EB%B6%80%EA%B8%B0%EC%A4%80&searchCnd=&searchWrd1=&pageNo=1");
pw.flush();
pw.close();
hConn.connect();
```
