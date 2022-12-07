---
layout: post
title:  "[Vue.js] Vue를 적용하니 IE11에서 하얀화면(blank)으로 나올때 2 "
categories: front 
comments: true

---





2편입니다.........................................

이걸 무슨 2편 할게 어딨다고 2편을 하는걸까요?^^^^

왜 저는 평생 IE와 싸워야하는걸까요...?

크롤러도 IE랑 전쟁을 하는데... 이제는 웹까지 전쟁준비를.......

... 왜 사람들은 아직도 IE를 이용하는걸까요..?



마소에서 그냥 IE서비스 중단할테니 엣지로 옮겨타라고 얘기해주면 한번에 끝날텐데

왜이렇게 뜸을 들일까요...........? 정말 IE는 악의 축임에 틀림없다!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

마소는 책임지고 죽여라.......

(저주)(저주)(저주)(저주)(저주)(저주)(저주)(저주)(저주)(저주)(저주)(저주)





2편을 쓰게된 이유는, 1편에서 했던 방식대로 `bael/polyfill` 을 잘 이용하는 것처럼 보였는데,

어느순간 보이는 `@babel/polyfill is deprecated. please use required parts of core-js`.

???????????????????????? 아니.. 

나 적용한지 한달도 안된거같은데 왜 deprecated래....... 그래서 뭐 똑같겠거니 core-js를 npm으로 설치했는데

아니... 왜 이제 크롬에서도 안나와....야....왜그래..........뭔가 디펜던시에서 문제가 생겼거나 뭐 그랬던것같다.



너무 당황해서 프로젝트를 다 지워버리고 새로 git clone으로 가져온뒤 npm i로 새로 한뒤 (다행히 새로 설치하고 나니 문제는 사라졌음) 크롬에서만 신나게(?) 작업했다



결국....크롬에서 어느정도 마무리 하고... 이제.. 크로스 브라우징을 해야할 순간이 점점 오기 시작했고...

결국 피할 수 없었다...ie너란 녀석.....흐앙



예전에 찾아가면서 봤던 수많은 글들을 다시 보고^^^^(다시보게 되다니 반갑구나?^^^)

그 내용으로 이미 한번 해결했으니 이제는 안보던 글을 봐야하는데, 안보던 글이 없다^^

IE 개발자도구를 사용해야하는 내 신세가 처량할뿐이지....



IE에서 개발자도구를 띄우고 내 vue프로젝트를 띄우면, 우선 기본적으로 `<div id="app"></div>`

안에 내가 작성한 모든 코드들이 dom에 찍혀야하는데!!!

심지어 `<!-- *built files will be auto injected* -->` 자동 삽입 된다며..... ie에서는 안된단 말이야....



하 정말 ie의 script 에러번호를 가지고 정말 온갖 github을 다 뒤져서(이번 문제는 오히려 stackoverflow에서는 없더라)

결국에는 core-js에서 issue로 등록되어있는 답변을 보고 답을 얻음 흑흑

(진짜 복받을 사람임...  좋아요 눌러주고싶은데 비활성화라 못누른게 한이네)





IE개발자도구에서 처음에는 무슨 `예외가 발생했지만 catch할 수 없습니다` 라면서 에러를 뿜길래 axios와 vuex를 알아먹지 못한다는 글이 떠오르면서 

혹시나 내가 axios뒤에 catch를 안붙여준게 예외사항을 못잡아 준건가 고민하며

300개가 넘는 catch를 다 붙이고 했는데도 안되는...ie....(난 정말 집착의 끝이 어딘지 가끔 궁금할때가...)

결국 catch의 문제가 아니었다.



`예외가 발생했지만 catch할 수 없습니다` 라는 거는 core-js의 internal-state.js안에 있는 펑션의 catch를 못잡아낸다는 뜻이었다... 내가 catch를 안적어서 그런게 아니고....... 



여태까지 문제상황만 길게 썼는데,

결국해결책은

`core-js` 를 설치할때,(1편에 나온것처럼 설정들은 똑같이 유지해주었음) 

`webpack:///node_modules/core-js/internals/internal-state.js` 경로에 보면 internal-state.js라는 js파일이 있다.

거기서 

~~~
var getterFor = function (TYPE) {
  return function (it) {
    var state;
  	 if (!isObject(it) || (state = get(it)).type !== TYPE) {
      throw TypeError('Incompatible receiver, ' + TYPE + ' required');
    } return state;
  };
};
~~~



`if (!isObject(it) || (state = get(it)).type !== TYPE) {` 이부분...!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

`removing the isObject(it) check in internal-state.js resolved the issue. The value of it is always a symbol and therefore always fails that check.`

*출처 https://github.com/zloirock/core-js/issues/514*



이라고 한다......그래서

~~~
if ( (state = get(it)).type !== TYPE) {
~~~

으로 바꿔주었더니....

나온다...^^..................................................................ㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎ

하 행복해........................................................

이제는.... 스타일 맞춰줘야하네...?

프리픽스프리로 한번 시도해봐야겠다.....







vue blank되는경우로 들어오시는 분들 요즘 많던데.... 이글보시고 시간단축하십쇼....







