---
layout: post
title:  "SCRIPT LANGUAGE=javascript FOR="
subtitle:   ""
categories: web
comments: true

---

<Script LANGUAGE = javascript FOR=MiPlatform Event = " FormSize()"> 는



~~~javascript
<SCRIPT LANGUAGE=javascript FOR=MiPlatformCtrl EVENT="FormSize(strSendFormID, lwidth, lheight)">
  //MiPlatformCtrl.CallScript("fn_resize('" + lwidth + "', '" + lheight + "')");
</SCRIPT>
~~~



마이크로소프트 확장 스크립트로 IE에서만 작동하는 스크립트이다. (미친놈들..)

없는게 좋은코드라고 사람들이 다들 그런다.. ActiveX에서만 작동하는거라고.... 미친놈들아ㅏㅏㅏㅏㅏㅏㅏ!!!



id 또는 name이 MiPlatformCtrl인 HTML 객체에 FormSize이벤트가 발생할경우 Script코드안의 JS가 실행되는 스크립트 코드라고 생각하면 된다. 

