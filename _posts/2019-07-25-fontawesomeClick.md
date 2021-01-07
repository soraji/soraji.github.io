---
layout: post
title:  "[JS] fontawesome icon 클릭 이벤트시 아이콘 변경"
categories: js
comments: true

---



요즘 많은 사이트들이 접었다 폈다 기능을 보여주려고 한다. 토글이 되면서 아이콘이 바뀌는 경우를 만들어야하는데 

fontawesome에서 아이콘을 많이 가져오니 i태그를 사용하려고 한다.

```html
<table width="100%" border="0" cellspacing="0" cellpadding="0" onClick="togglecheck(0);" style="cursor: pointer;" >
    <tr>
    	<td height="20">&nbsp;</td>
    	<td width="50"></td>
    </tr>
    <tr>
    	<td height="8"></td>
    	<td width="50"></td>
    </tr>
    <tr >
   		<td id="fold0" height="20" align="right"><i class="fas fa-chevron-down"></i></td> 
   		<td width="50"></td>
    </tr>
    <tr>
    	<td height="20"></td>
    	<td width="50"></td>
    </tr>
</table>
```



근데 접혀있을때는 아래로 화살표가 있지만 펼쳐져있을때는 위로 화살표가 있는 방향으로 되어야하는데 

img가 아니고 아이콘 태그이기때문에 getElementById를 사용해서 가져오도록 한다.

```javascript
document.getElementById("fold0").innerHTML = "<i class='fas fa-chevron-up'></i>";
```



이러면 토글이 될때마다 down / up 으로 아이콘이 변경된다.