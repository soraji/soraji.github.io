---
layout: post
title: "[ Java ] org/openqa/selenium/WebDriver : Unsupported major.minor version 52.0"
categories: error
comments: true
---

새로운 자바 프로젝트를 가지고 와서 집컴에 새로운 자바파일을 만들었는데,

갑자기 org/openqa/selenium/WebDriver : Unsupported major.minor version 52.0 이런 에러가 뜨는것이다...ㅇ0ㅇ...!!!!

보니까 지금 셀레니움에 문제가 있는거같은데.....

검색을 해보니 자바컴파일러가 버전이 맞지 않아서 발생하는 문제라고 한다.

이건, 정말 회사컴퓨터가 예전 버전을 사용하는 바람에 최신의 버전에서는 어떻게 적용하는지 1도 모른다는거다.. (내가 다하는거같네..)

우선 처음에도 32비트에서는 지원이 안된다는 에러만 뿜뿜하다가 지금은 내가 돌리는 JRE파일이 버전이 낮다는 말만 뿜어댄다.

자바폴더에서 properties에 들어가보면 java complier에 보면 그 버전을 선택하는게 있는데,

나는 분명 최신 1.8인데... 왜 안된다는 거지....

그렇담,

window에 preference-Installed JREs 에 들어가면 현재 설치된 JRE를 볼 수 있다.

처음에 java gui가 안떠서 Program Files(x86)\Java]jre를 Java32로 등록을 해놨는데,

이걸로 돌려도 버전안맞아서 컴파일안된다고 뜨길래,

이미 셀레니움을 지원하는 JRE가 있는 gongji의 JRE를 복사해서 C:에 다시 복사해주고,

Selenium을 지원해준다고해서 JavaS라고 이름붙인뒤 이 JREs로 컴파일을 시켜주었다.

실행시키려고 하는 패키지gui에서 run실행하기 전에 run configuration에서 JRE을 JavaS로 설정한뒤 실행한다.

(gongji파일에 JRE가 셀레니움 지원함)
