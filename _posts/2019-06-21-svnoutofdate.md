---
layout: post
title: "[ Java ] eclipse SVN commit 'is out of date' 오류 해결 방법"
categories: error
comments: true
---

svn을 쓰다가 어떤 파일을 삭제했더니 out of date 오류가 떴다.

Directory '파일경로' is out of date 뭐 이런식으로...

이럴때는

커밋하려고하는 파일에서 [team]-[update to HEAD] 눌러주면 싱크가 맞는다.
