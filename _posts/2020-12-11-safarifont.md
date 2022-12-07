---
layout: post
title:  "[css] safari에서 디폴트폰트가 명조로 나올때(font-family설정)"
subtitle:   ""
categories: front 
comments: true
---



safari브라우저에서 사이트를 들어가게 되면

분명 난 font family까지 다 설정해줬는데 한글이 명조로 나온다.

설정을 둘째치고 너무 보기 싫음

난 사파리를 안써서 (근데 개발자도구 보니까 은근 또 괜찮은것같아서 사파리도 고려중) 글씨가 이렇게 되는지 몰랐는데

맥에서 어떻게 하다가 들어가보니 명조로 뙇 !

모든 폰트가 다 명조로 나오는걸 보고..

와 이거 진짜 못생겼네 하고 폰트 설정도 다 해줬는데 왜 이러지? 싶어서

이것저것 찾아봤는데 역시나 역시나...

내 웹의 폰트는 돋움이다. 'dotum'이나 돋움.

Font-family특성상 컴마 순으로 폰트를 찾아서 렌더링을 하는데

맥의 safari에서는 돋움을 못찾아서 맥이 가지고 있는 폰트를 설정해주어야했다.

(결국이것도 뭐 나의 짧은 지식탓... 진작에 알았어야지...)

Apple Gothic이라고 하는데 놉놉!

진짜 폰트이름은 Apple SD Gothic Neo'이다.

그러니까 폰트 지정할때

` font-family:'dotum','돋움','Apple SD Gothic Neo';`

이렇게 써주면 된다.

처음에는 dotum을 찾고, 없으면 돋움을 찾고, 없으면 apple sd gothic neo를 찾는 식.



이런식이면 디폴트로 들어갔을때도 돋움체로 뜬다 =) 

해결!









