---
layout: post
title:  "ODBC를 이용한 node.js와 mssql연결"
subtitle:   "etc"
categories: back
comments: true
---



보통 수업을 듣거나 영상을 보면 대부분 예제, 아주 간단한 정도의 코드들만 실습하고 정말 실무에 배운것을 적용하려면 엄청난 삽질을 해야한다. 프레임워크를 이용해서 db에 연결을 하려고 하는데, 도대체가 다들 코드만 있고 제대로 된 정보는 왜 하나도 없는것이냐.. 그래서 오늘도 찌랭이는 적는다... ODBC로 mssql와 node.js연결하기.

node.js라는 플랫폼을 처음접해서, 이게 도대체 어디다 쓰는건지, 어떻게 써먹는건지 1도 몰랐던 나에게 node.js는 좀 짜증은 나지만, 새로운 세계를 열어준것 같았달까..? 하지만 갈길은 너무나 멀다. 너무나... 너무너무나.....

vue를 이용해서 구조를 잡거나, ui적인것은 알겠는데, 실제로 데이터를 연결하려고 하니 도대체 어디서부터 시작해야하는건가, 하는 질문이 끊이지 않았다. 다른 타회사를 보니 파이썬 장고를 이용하거나, php 라라벨을 이용해서 디비를 연결하던데, 우리는 그런거 없는데....? 대신 MS계열은 ODBC가 있다. 미들웨어 역할을 하는 중간장치가 있어야한다고 한다. 왜냐믄 디비를 다이렉트로 연결하는게 위험하다고 하니까..(왜 위험한지는 정확히 모른다. 아마 보안이라던지 db훼손위험이 있어서가 아닐까 싶긴한데..) db를 다이렉트로 불러오는게 위험한지도 어떤 다른 개발자분이 질문올린거 보다가 댓글달린거를 보고 'db연결을 바로하면 안되는거였네?' 라고 알게 되었다. 어쨌거나.. 사람들이 많이 mysql을 쓴다는데, 나는 mysql이 아니고 mssql임. sql계열이긴하지만 문법의 약간의 차이가 있는것 같다. 

(아직도 좀 궁금한게, node.js책을 사서 읽었는데 db와 연결하는 방법으로 시퀼라이저를 이용하면 연결이 가능하다고 했는데 require자체가 안먹는데 도대체 어떻게 연결한다는건지 모르겠다. node에서는 require을 알아듣는데 브라우저에서는 require을 못알아듣는다고 해서 browserify, requirejs도 엄청 알아봤는데 결국은 해결하지 못하고 캡팡강사님께 질문드림. 그리고 알아낸 사실하나..!)

![email](/assets/img/email.PNG)

>require() 문법으로 NPM 모듈을 불러올 수 있는 건 맞지만,
>
>MySQL과 같은 데이터베이스 모듈은 프런트엔드에서 불러올 수 없습니다.
>
>
>
>기본적으로 NPM 모듈은 프런트엔드용 모듈과, 서버용 모듈로 나뉘어져 있어요.
>
>예를 들어, jQuery와 같은 웹 라이브러리는 require()로 로딩이 되지만,
>
>mysql, mongodb와 같은 서버용 모듈은 웹팩으로 빌드할 수 없습니다.

 ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ )  ( •᷄⌓•᷅ ) 무ㅓ시라구요????

연결이 안된다굽쇼 ( •᷄⌓•᷅ ) ? 흠 🤔 위험해서 그런걸까 안된다니 안된다는거겠지만..

그래서 다른 방법을 찾아야 했다. 여태까지는 그냥 다이렉트로 연결하는 방법을 찾았던건데 이제는 중간에 미들웨어를 하나끼고 연결하는 방법을 찾아보았다. ODBC로 연결하기.

[Create Node.js apps using SQL Server on Windows](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/windows/) 야.. 너 진심 엄청 찾았음… 왜 이제야 나타나냐……..

# 1단계

---

## Install Chocolatey and Node.js

> If you already have Node.js and choco installed on your machine, skip this step. Install Chocolatey using this command in an elevated Command prompt (run as administrator).
>
> 만약 당신의 기계에 이미 node.js와 choco가 설치되어있다면, 이 단계는 건너뛰세요. Chocolatey를 설치하려면 아래 커맨드프롬프트의 명령어를 사용하세요(관리자 권한으로 cmd를 실행하세요)

~~~
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
~~~

> For choco to work, you now need to restart the terminal session by closing and opening the command prompt. Open an elevated Command prompt (run as administrator) and run the following commands to install node:
>
> Choco가 작동하게 하려면, 터미널세션을 닫고 엶으로서 재시작을 해야할 필요가 있습니다. cmd 프롬프트를 관리자 권한으로 실행후 node설치를 위해 아래 명령어를 실행하세요

~~~
choco install -y nodejs
~~~

그렇다면 결과는 😀 아래처럼 나올텐데 엄청엄청 길게 주주주주주죽 다운될것이다

~~~
Installing the following packages:
nodejs
The install of nodejs was successful.
  Software install location not explicitly set, could be in package or
  default install location if installer.

Chocolatey installed 1/1 packages. 0 packages failed.
 See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
~~~

설치완료! You have succesfuly installed Node.js and Chocolatey on your machine!! (Chocolatey설치하라고 해서 설치는 했다만 도대체 저게 뭔지 모르겠다...)



---

## Install sqlcmd - A Command Line Utility for SQL Server

이제 실제로 SQL server에서 쿼리명령어를 알아듣게 도와주는 sqlcmd를 설치해야한다. 

> SQLCMD is a command line tool that enables you to connect to SQL Server and run queries.
>
> SQLCMD는 SQL에 연결을 가능하게하고 쿼리문을 실행시키는것을 가능하게 해주는 cmd명령어 툴이다.

하지만 이 SQLCMD를 설치하려면 [ODBC드라이버](https://www.microsoft.com/en-us/download/details.aspx?id=36434)를 먼저 설치해야한다. 최신버전이 17이라 17을 깔았는데 [Microsoft® Command Line Utilities 14.0 for SQL Server®](https://www.microsoft.com/en-us/download/details.aspx?id=53591) 에서 계속 11이 없다고, ODBC11이 없다고!!!! 계속해서 설치가 안된다고해서 나는 11을 깔았다. (ODBC드라이버를 링크해놓은것은 11버전으로 링크걸어놨다)

> After installing SQLCMD using the msi’s, you can connect to SQL Server using the following command from a CMD session:
>
> SQLCMD를 msi로 설치한후(msi는 윈도우의 exe같은것)에는, 아래의 cmd로 SQL 서버에 연결할 수 있을것이다. 

~~~
sqlcmd -S localhost -U sa -P your_password
1> # You're connected! Type your T-SQL statements here. Use the keyword 'GO' to execute each batch of statements.
~~~



자, 보면 sqlcmd는 당연히 명령어이고 -S 는 연결하고자하는 ip, -U는 사용자,  -P는 비밀번호이다. (sa가 사용자인지 첨 알았음)

제대로 입력하고 엔터를 누르면 1> 이라고 뜬다. 이러면 잘 접속된거임!! 🎉🎉축하축하🎉🎉

(나는 처음에 ip는 맞는데 다른 사용자와 비밀번호를 계속 넣어서 사용자'<사용자이름>'이(가) 로그인하지 못했습니다 오류가 계속해서 뜸.... 확인잘 해보고 아이피, 사용자, 비번이 제대로 일치하는지 잘 확인할것!!)

---

# 2단계

나는 새로 db를 만들어서 내가 db를 insert하는게 아니고, 이미 있는 db를 연결해서, 그 안에 있는 데이터를 select해줘야하는데 다들 죄다 알려주는글들은 db생성부터 알려줘서 나랑은 관련없는 글들이었다 ㅠㅠ 근데 ms에서 웬일로 이렇게 친절하게 다 알려주는 글이 있다니 칭찬해주고싶꾼! 여튼 그래서 우선 지금 db에 연결이 되었다. 그럼 제대로 db에서 가져오는지 보기 위해서 테스트로 간단한 쿼리문을 날려보자. 

~~~
select top 10 * from customer
~~~



이렇게 치고 엔터를 치면 2>라고 뜬다. 
???????결과값이 나와야지 왜 2>가 뜬다냐..?
알고봤더니 저 위에 Use the keyword 'GO' to execute each batch of statements. 라고 있듯이 GO를 써주어야한다. 2>에 GO를 쓰고 엔터! 그러면 cmd에 촤르르르르륵하면서 db에 있는 내용을 뿌려준다. 회사내 정보라 사진이 없지만, 있다고 해서 뭐 별로 가독성이 좋지는 않다.cmd라 그런지 아주 가독성이 기가찬다. 뭔내용인지 판독불가능임. 그냥 회원이름만 확인해서 정보가 맞는구나 확인하는정도? 어쨌거나 프론트랑 백이랑 연결하는 법을 터득했다!



---

# 3단계

근데 내가 작업하려고 하는 드라이브는 D:인데, sqlcmd는 C:에만 깔려있는것 같다! D:에서 sqlcmd라고 명령어를 치니까 알 수 없는 명령어라고 한다..! 그렇담 path를 정해줘야지..!!!

보통 sqlcmd가 디폴트로 설치되는 경로는 c:\program Files\Microsoft SQL Server\Client SDK\ODBC\110\Tools\Binn에 SQLCMD.EXE가 있다. 

![img](/assets/img/180911001.png)

그럼 그 파일경로를 복사해서, 컴퓨터에서 마우스 오른쪽누르고 속성에 들어가면 제어판의 시스템창이 뜨는데 

![img](/assets/img/180911002.png)

그곳에서 고급 시스템설정->고급 탭->환경변수->

![img](/assets/img/180911003.png)

PATH변수를 클릭하고 편집->

![img](/assets/img/180911004.png)

그리고 맨뒤로 가서(End키보드) -> 세미콜론치고(;) 복사한 경로를 붙여준다.->그러면 아마 ';c:\program Files\Microsoft SQL Server\Client SDK\ODBC\110\Tools\Binn' 이렇게 쳐질텐데 이 뒤에 SQLCMD.EXE라고 쳐주면 path설정 완료이다. 설정한뒤에

![img](/assets/img/180911005.png)

D:에서 SQLCMD명령어 내리니 잘 된다.





이렇게 ODBC를 이용해서 mssql과 node.js를 연결해봤다. 근데 이제 이 데이터들을 가지고와서 자유자재로 하는건 다음step부터 진짜 시작인것 같다. 그건 다음에 해봐야지! 연결만 한것도 아주아주 대단타!



