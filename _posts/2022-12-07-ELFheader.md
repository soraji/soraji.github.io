---
layout: post
title:  "Error: /myfolder/node_modules/bcrypt/lib/binding/napi-v3/bcrypt_lib.node: invalid ELF header"
categories: error
comments: true
---





`Error: /myfolder/node_modules/bcrypt/lib/binding/napi-v3/bcrypt_lib.node: invalid ELF header`

에러가 떴다.

스택오버플로우보니까 node_modules가 없어서 그런거라고 그러는데....

그게 아니고 난 도커로 띄우는데 `.dockerignore` 파일이 없어서 그런거였다...( ༎ຶŎ༎ຶ )



.dockerignore 파일 만들어주고 그 안에다 node_modules랑 dist 넣어주고 돌렸더니 에러 해결!

`invalid ELF header` 글자만 나와도 이제는 확실하게 기억한다...!