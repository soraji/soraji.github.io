---
layout: post
title:  "[Vue.js] Vue ref | ref로 vue child component의 function 컨트롤하기(뷰 자식컴포넌트 함수 실행하기)"
categories: front 
comments: true
---



vue에 올인한지 벌써 꽤 오랜시간이 지났다......

어느정도 뷰의 기능을 막힘없이 사용할 수 있게된것같다...........................

삽질을 너무 많이했어 흑흑



오늘은

부모 자식컴포넌트간의 데이터통신에 대해서 알아보도록하자

보통 부모가 자식에게 데이터를 넘겨주고 이것을 props라고 한다.

자식은 부모에게 메소드를 넘겨주고 이것을 event라고 한다.

(이거 헷갈리는 개념이라 계속해서 뚫어지게 쳐다보고 완벽히 이해해야한다.)



근데 그러면.... 부모가 자식에게 메소드를 넘기고싶을때는? (ref를 사용합니다)

자식이 부모에게 데이터를 넘겨주고 싶을때는? (emit에서 payload를 사용합니다)

부모와 자식이 양방향으로 완벽하게 데이터와 메소드를 넘기고 싶을때는 어떻게 할까?

이때는 refs를 사용하면 된다.



vue를 많이 해본사람들은 이미 알겠지만, vue에서는 refs라는 속성을 통해서 

우리가 흔히 이용하는 getElementsByClass라던지, getElementById라던지 등등 쿼리셀렉터의 기능을 대신해준다.

그래서 만약 id가 unique라는 엘리먼트가 있으면

~~~
<div id="unique">난 유니크해</div>
~~~

우리는 JS로 엘리먼트에 접근할때 getElementByID("unique") 이런식으로 사용하지만

vue에서는 refs를 사용하는것을 권장한다. 



[어떤분](https://devriver.tistory.com/31)이 (지금은 휴면계정이라 댓글도 남길수없다지만) vue에서의 refs는 가급적 권장하지 않는다고 써놓은 글이 상위에

노출이 되어서, 잘못된 정보를 접하는 사람들이 있는 것 같다.

사실 나도 그래서 ref를 권장하지 않는지에 대한 내용을 찾아보기도 했었다.

하지만 그런내용은 없었고 모두가 다 ref를 권장하고 있다.

직접적인 DOM조작을 할 필요가 없고, 훨씬 깔끔하기 때문이다.



여튼간 vue를 사용한다면 쿼리셀렉터보다는 refs를 권장하는데

엘리먼트들에 접근하거나, 부모가 자식에게 접근하기 위해서 정말 간편한 방법이 아닌가 싶다.

vue에서 부모컴포넌트가 자식 컴포넌트의 메소드에 접근하고 싶을떄가 있다.

그럴때는 이때도 ref를 쓰면 된다.

예를들어

parent라는 부모cmp가 있고, child라는 자식cmp가 있다고 해보자

그러면 parent.vue안에 child.vue를 import 해주고 컴포넌트 등록까지 다 마쳐준다.

그리고 그 자식cmp에 ref="원하는 이름"을 써준다.(나는 child로 동일하게 사용함)

~~~vue
//parent.vue
<template>
	<child ref="child"></child>
</template>
<script>
import child from './child.vue';
export default {
  components:{
	child
  },
  mounted(){
      this.$refs.child.functionYouWantToCall();
  }
}
</script>
~~~



그리고 나서는 mounted에 this.$refs.child로 child컴포넌트에 접근이 가능하도록 한다.

(여기서 주의할점. $refs는 dom에 렌더링이 다 된 이후에 접근 가능하다. 

그러니까 created가 아니라 mounted에서 실행해야한다. created에서 쓰면 undefined뜰거임

가끔 이것도 모자라서 mounted했는데도 undefined뜰때가 있음. 이때는 setTimeOut으로.....

그리고 템플릿에서는 ref지만 js에서 접근할때는 refs 다)



이렇게 컴포넌트에 접근한 이후에 원하는 함수명을 써주면 된다.

어떤 함수가 있는지 모르겠지만 그냥 functionYouWantToCall() 이라는 메소드가 있다고 치고,

~~~vue
this.$refs.child.functionYouWantToCall();
~~~

이렇게 해주면

부모컴포넌트에서 자식컴포넌트에 있는 메소드까지 직접적으로 접근 및 실행이 가능하다!

굉장히 유용한 기능이니 많이 많이 알았으면 좋겠다