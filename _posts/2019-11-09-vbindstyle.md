---
layout: post
title:  "[Vue.js] v-bind:style (스타일, 변수 바인딩해주기)"
categories: front 
comments: true
---





스타일을 바인딩해주려면

```vue
v-bind:style="{color:'#000000'}"
```

이런식으로 써주면 되는데,



만약 스타일도 조건을 걸고 싶다면 

```
v-bind:style="[ 조건 ? {color:'#000'} : {color:'#fff'} ]"
```

으로 걸면 된다.

저러면 조건이 참이면 앞의 스타일이, 거짓이면 뒤의 스타일이 적용된다.



※스타일 뿐만 아니라 변수를 바꾸는데도 똑같이 사용할수있다. bind는 아니지만 if와 `?` `:`를 이용해서 

내가 원하는대로 수정할수있다.

```vue
<!-- for문에서 돌아가는 중 this.$store.state.poodles에서 도는 중-->
<td rowspan="2">
    <div v-if="poodle.name == '세라' ? poddle.name = '막내' : poodle.name"></div> 
    <div v-if="poodle.nick == '똥강아지' ? poodle.nick = '막내' : poodle.nick"></div>
    <div v-if="poddle.name == poddle.nick" >
        {{poodle.name}}
    </div>
    <div v-else>
        <table>
            <tr>
                <td>{{poodle.name}}</td>      
            </tr>
            <tr>
                <td>{{corgi.name}}</td>
            </tr>
        </table>
    </div>
</td>
```

위 코드는 

1) 만약 받아온 데이터의 name이 '세라'와 일치하면 세라 대신 막내를 사용하고, 일치하지 않으면 그냥 원래 데이터 이름을 사용한다.

2) 만약 받아온 데이터의 nick이 '똥강아지'와 일치하면 똥강아지 대신 막내를 사용하고, 일치하지 않으면 원래 데이터 이름을 사용한다.

3) 만약 데이터의 name과 nick이 같다면 하나만 표시해주고, 같지않다면 한칸에 위아래로 나눠서 적어주도록 한다.





asp에서 항상 이렇게 썼어서 편하긴했지만 가독성이 떨어졌었는데.. 뷰는 asp보다 가독성은 더 나은거같지만

혹시 다른방법이 있을까.. 알아보는중

