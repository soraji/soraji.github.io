---
layout: post
title:  "[Vue.js] #2.레거시 위에 vue.js 올리기(IIS에서 node.js (vue.js) 연동하기)"
categories: front 
comments: true
---

*참고글*

1번. [api를 만들어 어떻게 json형식으로 데이터를 가져올것인가](https://soraji.github.io/back/2019/09/25/classASPjson/)

2번. [vue의 코드들을 어떻게 웹서버에서 올릴것인가](https://soraji.github.io/front/2019/10/31/vuewithlegacy1/)



이번에는 3번. 노드서버에서(localhost:8080) 돌리는 vue를 어떻게 IIS와 연동할것인가 를 적어본다.



vue든, react든 다 자바스크립트 프레임워크들이므로 IIS와 node.js만 연동해놓는다면 아마 비슷비슷하지 않을까..하는 생각이 들지만, 나는 vue를 쓸것이므로 vue로 예제 진행.

IIS가 서버가 될것이므로 IIS에 세팅이 필요하다.



### IIS 최상위서버 기본설정

IIS에 설정을 하기 위한 카테고리?를 만들려면 2가지를 다운받아야한다. 기본으로 설정했을때는 없었다(iis를 설정하다보면 없는 카테고리들이 몇몇 있는데, 그럴때 보면 항상 체크를 안하고 넘어갔던가 하는 뭐 그런문제이다 예전에 ASP카테고리가 그랬음!)

첫번째, Application Request Routing 를 설치해야한다. [다운로드](https://www.microsoft.com/en-us/download/details.aspx?id=47333) (설치하고 진행해보니 얘는 딱히 없어도 잘 돌아간다.)

두번째, URLRewrite 를 설치해야한다. [다운로드](https://www.iis.net/downloads/microsoft/url-rewrite)

두개 다 설치하면 각 사이트에서가 아닌 'WIN-어쩌구저쩌구 홈' 에 들어가면 'Application Request Routing Cache'가 생기는데 

ARR아이콘을 더블클릭해서 들어가게 되면 우측에 'Server Proxy Settings...' 를 클릭하고 가장 상단의 'Enable proxy'체크박스를 체크해서 프록시를 허용해주어야한다.

그리고 그 WIN어쩌구에서 URL재작성 이라는 아이콘이 있는데 그 아이콘을 누른다.(각 사이트마다도 URL이 있는데 거기서 하면 안됨! 가장 상위 홈에서 해줘야함)

들어가면 우측에 [규칙추가]를 눌러 새로운 인바운드 규칙을 만들어준다.

이름은 마음대로하고,

[URL검색]에서 '요청한 URL'은 '패턴과 일치'로, '사용'은 '와일드카드'로, '패턴'은 모든것을 허용한다는 뜻의 '*' 로 작성한다.

그리고 밑에 가면 [작업]이라고 있는데 

'작업유형'에서 '재작성'을 선택하게 되면 '작업속성'이라면 밑에 내용이 추가된다.

'URL재작성'이라는 곳에 내가 원하는 도메인을 적어준다. 

이부분이 주의해야할 부분인데, 

현재 작업하고 있는 위치는 가장 상위 단위의 홈이기 때문에 

서버가 가지고있는 모든 사이트주소에 하나라도 URL을 가지고 있으면 가능하게 해주는것이다.

예를들어 내가 사이트를 특별하게 지정해주지 않았다면 사이트에서 사이트를 띄우면 localhost로 뜰것이다.

그런데 만약 사이트를 지정해주었으면 'node.com'이런식으로 사이트에 지정된 사이트를 적어준다. 그리고 포트와 {R:1}도 적어주어야한다.

그러니까 최종적으로는

```
내가지정해준호스트이름:포트번호/{R:1}
//예를들자면 test.co.kr:80/{R:1} 이런식..
```

이 되는것이다.





rewrite에 인바운드 규칙을 등록해도 되지만 웹사이트와 연동하면 자동생성되는 web.config파일을 수정해주어도 되는것 같다.

인바운드규칙을 생성하니 저절로 config안에 내용이 생겼다.

하지만 그래도 config안에 내용 적어줄랭..

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Handle History Mode and custom 404/500" stopProcessing="true">
            <match url="(.*)" />
            <conditions logicalGrouping="MatchAll">
              <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
              <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
            </conditions>
          <action type="Rewrite" url="index.html" />
        </rule>
      </rules>
    </rewrite>
      <httpErrors>     
          <remove statusCode="404" subStatusCode="-1" />                
          <remove statusCode="500" subStatusCode="-1" />
          <error statusCode="404" path="/survey/notfound" responseMode="ExecuteURL" />                
          <error statusCode="500" path="/survey/error" responseMode="ExecuteURL" />
      </httpErrors>
      <modules runAllManagedModulesForAllRequests="true"/>
  </system.webServer>
</configuration>
~~~

반대로 web.config에 내용을 적어주어도 자동으로 url rewrite에 규칙이 생성되어있다.



호스트이름은 test.co.kr로 지정하고 IP는 지정하지 않은 모든 IP, 포트는 80으로 한다.

(아까 URL재작성에서 그러면 test.co.kr:80/{R:1} 로 지정해주면 된다.)



그리고 Hosts파일에 등록해준다.

그러면 hosts를 등록해준 다른 피씨에서도 접속가능! 아직 도메인은 구매하지 않았다고 가정한것이므로 hosts등록해준곳들만 접속가능하다



근데 갑자기 ie에서 'javascript가 enable'되어있지 않으므로 doesn't work property라고 떴다...! 분명 접속은 되는데 왜저러는거지...! 확인해봤더니 브라우저문제였다. ie인터넷옵션에서 설정해주면 된다.

[여기서 확인](https://www.whatismybrowser.com/guides/how-to-enable-javascript/internet-explorer)







