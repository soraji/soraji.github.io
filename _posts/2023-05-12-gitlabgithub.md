---
layout: post
title:  "한 컴퓨터(mac)에서 Gitlab과 Github에 각각 계정 등록하기"
subtitle:   ""
categories: back
comments: true
---



<br>

<br>

<br>

회사는 gitlab을 사용하고 내 개인계정(블로그)는 github을 사용하기 때문에 두개의 각각의 처리를 해줘야 했다.

git을 사용하는건 똑같은데 계정이 다르니까 gitlab으로 어떤 작업을 하든간에 매번 로그인해야했고,

github을 사용하려면 gitlab계정이라 push가 되긴되는데 잔디가 안찍히고.. 양쪽다 번거로운 작업을 계속해서 해줘야 하는 상황이었다.

특히 gitlab같은 경우는 2FA(2단계인증)를 사용하느라 더더욱 계정을 등록할 필요가 있었다.

vscode에서 작업할 때마다 매번 아이디랑 비번을 물어보니 진짜 짜증이 어마무시하게 났다. 

아직까지는 별로 작업할 일이 없어서 몇 번 안됐지만, 

만약 본격적으로 작업하게 되면 진짜 진행이 안될 수도 있겠다는 생각이 들어서 그냥 언능 후딱 진행하기로 했다.

근데 안되서 진짜 이런저런 정보 엄청 찾고 동료도움받고.. 결국엔 해결해서 기록해놓는다.

<br>

<br>

## SSH keys 생성

SSH로 등록을 해주어야 하기 때문에 각각의 계정을 등록하는 SSH를 만들어야 한다.

~~~
ssh-keygen -t rsa -C "깃랩계정이메일" -f ~/.ssh/id_rsa_gitlab
ssh-keygen -t rsa -C "깃헙계정이메일" -f ~/.ssh/id_rsa_github
~~~

<br>

위의 코드를 명령하면 이름을 뭐로할건지, 비번을 뭐로 할건지 등등을 물어보는데 난 아무것도 설정안하고 그냥 다 빈칸빈칸으로 진행했고

(설정하면 그 설정 값을 매번 ssh 볼때마다 입력해줘야하는걸로 안다.. 귀찮)

<br>

이렇게 등록하면 `~/.ssh` 경로에

~~~
~/.ssh/id_rsa_github
~/.ssh/id_rsa_github.pub
~/.ssh/id_rsa_gitlab
~/.ssh/id_rsa_gitlab.pub
~~~

이렇게 ssh 키가 각각 생성되는 것을 확인할 수 있다.

<br>

<br>

`cat id_rsa_gitlab.pub` 으로 조회한 ssh key를 복사해서 gitlab(User Settings > SSH Keys) SSH를 등록하고 

github(setting > SSH and GPG keys)도 동일한 방법으로 등록한다.

<br>

<br>

## SSH Keys 로컬에 등록하기

~~~
ssh-add ~/.ssh/id_rsa_github
ssh-add ~/.ssh/id_rsa_gitlab
~~~

를 명령해서 ssh-agent를 이용해서 로컬에 등록해준다

<br>

<br>

## Config 파일 수정

`~/.ssh` 경로에서 `vi config` 를 하면 config를 수정할 수 있는데

~~~
#GitLab
Host gitlab.com
	HostName gitlab.com
    	User git
    	IdentityFile /Users/soraji/.ssh/id_rsa_gitlab
#GitHub
Host github.com
	HostName github.com
	User git
	IdentityFile /Users/soraji/.ssh/id_rsa_github
~~~

아래처럼 각각 별도의 config를 설정해준다.

<br>

<br>

이러면 깃랩 깃헙 둘다 하나의 컴퓨터에서 다른 계정으로 등록, 사용가능하다!

<br>

<br>

근데 이렇게 보니까 엄청 간단하네

이거 해결하느라 엄청 오래 걸렸는데...

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>



*참조 : https://medium.com/@viviennediegoencarnacion/manage-github-and-gitlab-accounts-on-single-machine-with-ssh-keys-on-mac-43fda49b7c8d*







