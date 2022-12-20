---
layout: post
title:  "error getting credentials - err: docker-credential-gcloud resolves to executable in current directory(docker-compose.prod.yaml push 중 에러)"
categories: error
comments: true
---





`error getting credentials - err: docker-credential-gcloud resolves to executable in current directory` 에러가 떴다



도대체 패스도 연결이 잘 되고, 다른거 연결이 다 잘되는데.. 

push 가 안되면 vscode 껐다 키래서 껏다켰다 5번 넘게하고 컴퓨터도 재부팅도 했는데 안되고..... 

도대체 뭐가 문제인가 했더니

<br>

<br>

내가 맨 처음에 `bash` 인줄 알고 만들었던 `~/.bashrc`  파일이랑 `~/.zshrc` 이 2중으로 읽혀서 그랬던것 같다...!

~~~
$ echo $SHELL
~~~

로 본인의 shell이 뭔지 확인한다

`/bin/zsh` : `zsh` 가 본인의 shell

`/bin/bash` : `bash` 가 본인의 shell

(나는 처음에 bash가 내 shell인줄 알았지 (  ˊ࿁ˋ ) ᐝ....)

그래서 

~~~
$ rm -rf ~/.bashrc
~~~

로 `~/.bashrc` 를 삭제해주고 

내가 도커를 돌릴 폴더에 들어가서

~~~
$ docker-compose -f docker-compose.prod.yaml push 
~~~

를 돌려주니 되었다





