---
layout: post
title:  "[Vue.js] #3.레거시 위에 vue.js 올리기(레거시에서 window.open을 했을때 vue 빌드 부르기)"
categories: front 
comments: true

---



*참고글*

1번. [api를 만들어 어떻게 json형식으로 데이터를 가져올것인가](https://soraji.github.io/back/2019/09/25/classASPjson/)

2번. [vue의 코드들을 어떻게 웹서버에서 올릴것인가](https://soraji.github.io/front/2019/10/31/vuewithlegacy1/)

3번. [노드서버에서(localhost:8080) 돌리는 vue를 어떻게 IIS와 연동할것인가](<https://soraji.github.io/front/2019/10/31/vuewithlegacy2/>)





오늘은 4번 주제.

## 레거시에서 window.open을 했을때 vue 빌드 부르기

에 대해서 적는다.



보통은 vue로 웹페이지를 만들면 쭉 vue로만 만들기 때문에 레거시에서 부를일이 없겠지만.. 상황은 항상 뜻대로 되지는 않으니..ㅜ.ㅜ

레거시에서 window.open으로 새창을 띄웠을때 vue로 만든 창이 떴으면 좋겠는데 그걸 하는 방법을 알아보겠다.

꼭 window.open이 아니더라도, href라던지, a태그라던지.. 뭐 여튼 페이지 이동을 위해서 vue의 경로를 지정해줄때

보통은 .jsp /.asp/ .php /.html 등등으로 끝나는 파일들이 프레임워크를 사용하면 (라우터를 사용하게 되니) 

path로 끝나게 되니 보통은 단어로 끝나서 어떻게 경로를 적어주어야하는지 알수가 없었다.

(나만 알수없었던 건가.. 난 진짜 엄청 고민했었는뎈ㅋㅋㅋ)



테스트해봤더니

~~~
-root
	-html
		-dirA
		-dirB
		-dir3
		-dist
~~~

이런식의 파일구조라면 그냥 항상 해주었던것처럼 

~~~
호스트이름/dist/라우트명
//ex) https://test.com/dist/라우트명(혹은 index.html)
~~~

이렇게 적어주면 된다.



처음에는 index.html을 적어주었는데 그거는 어떠한 라우터도 지정해주지 않았을때 디폴트로 가는 페이지라,

만약 디폴트가 어떤 라우터의 모습으로 바로 보여지는걸 원한다면 라우터 명을 쓰면 된다.

그리고 이 dist폴더는 빌드했던 dist폴더와 동일하게 넣어주면 된다.



그런데 문제는 빌드를 하게 되면 디폴트는 index.html의 내용에 루트로 기본 경로가 잡히게 된다는 건데,

그렇게 되니 빌드를 하고 파일을 옮길때마다 index.html을 수정해서 /dist/css/xxxx.css로 모든 js와 css와 ico의 경로를 다 바꿔주어야 했다.

어지간히 귀찮은 일이어야지 말이다...

나는 루트가 아니고 옮겼을때 바로 그 경로가 맞았으면 했다.



빌드 옵션을 지정해주면 되는 건데, 이건 vue.config.js에서 하면된다. (난 vue cli3를 사용한다)

(vue.config.js는 새로운 파일로 만들어주었다. 디폴트에는 없다. cli2에는 webpack.config.js가 있었던걸로 기억하는데..잘모르겠다)

~~~
module.exports = {
  lintOnSave : false,
  publicPath: 'dist/',
  assetsDir: process.env.BASE_URL 
}
~~~

이렇게 지정해주었는데,

이렇게 되면 빌드가 되고 나서는 index.html의 내용을 보면 css, js의 내용은 /dist/css/xxx.css /dist/js/xxx/js로 dist경로를 기준으로 생성되고,

index.html는 /dist/index.html로 생성된다.

public은 index.html

assets은 css,js 형성 경로에 대해서 지정을 해주는것이다.(css와 js는 빌드할때 몇개의 파일이 생성되기 때문에 css디렉토리/js디렉토리에 따로 생성되기 때문이다)



아무튼, 이렇게 경로지정해주니까 옮길때 경로안바꾸고 바로 사용할수있어서 편하다!

