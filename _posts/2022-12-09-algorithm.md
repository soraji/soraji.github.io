---
layout: post
title:  "후위연산식 (stack개념)"
categories: algo
comments: true
---

<br>

<br>

후위연산식이 주어지면 연산한 결과를 출력하는 프로그램을 작성하세요.

만약 3*(5+2)-9 을 후위연산식으로 표현하면 352+*9- 로 표현되며 그 결과는 12입니다.

▣ 입력예제 352+*9-

▣ 출력예제 12

<br>

<br>

<br>

<개념 & 문제 풀이법 생각>

후위연산식이란 숫자는 앞으로, 연산자는 뒤로 빼주는 연산식을 말한다.

예를들어 `5+3` 이 있다면, `53+` 으로,

`5-3`이 있다면 `53-` 로 바꿔줄 수 있다.

여기서 연산자 기준으로 위치가 중요하다. 앞의 5가 연산을 당하는 입장이다.

그래서 후위연산식으로 만들어주려면 str으로 넘어온 식을 for문으로 돌리고

숫자일때는 push, 연산자일때는 좀 생각을 해봐야한다.

왜냐면 이 와중에 `+` `*` 는 상관이 없지만 `-` `/` 는 순서가 중요하기 때문에 

어느쪽이 연산자 기준 왼쪽 오른쪽인지가 중요하다.

(헷갈리지 않게 왼쪽은 lt로, 오른쪽은 rt로 선언해주기)

그래서 연산자를 만나는순간, pop으로 2개의 숫자를 뽑는다.

그리고 첫번째로 나오는 숫자를 rt에 넣고,

두번째로 나오는 숫자를 lt에 넣는다

그리고 연산이 끝난 숫자를 다시 stack에 넣는다 * 반복

<br>

<br>

<br>


---

# CASE 1

~~~js
function solution(s){  
	let answer;
	let stack=[];
	for(let x of s){
		//isNaN(x) : x가 숫자가 아니면 참, 숫자면 거짓
		//!isNaN(x)) : x가 숫자일때 넣어야 하므로 앞에 !로 결과값뒤집어주기
		if(!isNaN(x)) stack.push(Number(x));	//숫자면 push, string으로 넘어오기 때문에 연산을 해주려면 Number처리
		else{	//숫자가 아니면 = 연산자라면
			let rt=stack.pop();	//제일 위에있는 숫자 꺼내기
			let lt=stack.pop();	//그 다음 위에 있는 숫자 꺼내기

			//일일히 다 해줘야함
			if(x==='+') stack.push(lt+rt);
			else if(x==='-') stack.push(lt-rt);
			else if(x==='*') stack.push(lt*rt);
			else if(x==='/') stack.push(lt/rt);
		}
	}
	answer=stack[0];
	return answer;
}

~~~

---

<br>



<br>



<br>

<br>

<br>

<br>

출처 :  [자바스크립트 알고리즘 문제풀이 입문(코딩테스트 대비)](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4/dashboard)를 보고 작성!

