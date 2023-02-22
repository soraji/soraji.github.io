---
layout: post
title: "[ Java ] selenium IE : NoSuchWindowException : Unable to get browser에러"
categories: error
comments: true
---

org.openqa.selenium.NoSuchWindowException: Unable to get browser

Driver info: org.openqa.selenium.ie.InternetExplorerDriver

IE셀레니움을 쓰는데(하 진짜 안쓰고싶음)

browser에 접근할수없다고 에러가 뜨면 IE에 인터넷옵션으로 들어간 뒤,

[보안] 탭의 보안수준을 똑같이 맞춰주어야한다.

[인터넷]과 [로컬 인트라넷]과 [신뢰할수있는 사이트]와 [제한된 사이트] 4개의

'이 영역에 적용할 보안 수준'을 체크하려면 4개 다 체크하고, 해제하려면 4개 다 해제해야함
