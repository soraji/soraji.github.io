---
layout: post
title:  "[JS] JavaScript format Number (천단위에 콤마, 처리)"
categories: js 
comments: true


---

<br>

<Br>

자바스크립트에서 천단위로 ,(콤마)를 찍어주는 방법에는

function도 있겠지만

~~~javascript
function formatNumber(number)
{
    number = number.toFixed(2) + '';
    x = number.split('.');
    x1 = x[0];
    x2 = x.length > 1 ? '.' + x[1] : '';
    var rgx = /(\d+)(\d{3})/;
    while (rgx.test(x1)) {
        x1 = x1.replace(rgx, '$1' + ',' + '$2');
    }
    return x1 + x2;
}
~~~

<br>

<Br>

나는 그냥 간단하게 화살표함수 안에다가 사용하고 싶었다.

그때는 그냥 `.replace(/(.)(?=(\d{3})+$)/g,'$1,')` 을 써주면 된다





참고 https://stackoverflow.com/questions/5731193/how-to-format-numbers

<br>

<Br>

<Br>

<br>

<Br>











