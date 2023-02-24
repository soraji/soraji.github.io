---
layout: post
title: "[ Python ] 파이썬 오프셋 인덱스"
categories: back
comments: true
---

```
data 변수를 선언하여 문자열 오프셋을 이용하여 (data = "0123456789") 짝수만 거꾸로 출력되도록 코드를 작성하세요.(86420)
```

#### offset index

- iterable[index]
- iterable[start:end]
- iterable[start​: end: ​stride] ( stride는 보폭, 간격 이라고 생각하면 된다.)

offset index :

​ 문자열을 작성하게되면, 각 문자마다 고유의 번호가 매겨지는데 이 번호를 `오프셋`이라 하고,

​ 오프셋을 이용하여 문자열에서 문자를 추출하는 것을 `인덱싱`이라 한다.

```
data = "0123456789"

# TODO
data1 = data[::2]
print(data1[::-1])
```

start와 end에는 값이 없고 stride에만 값이 있다(2). 그러므로 2칸 보폭으로 02468이 출력되는데 다시 -1보폭으로 출력하니 거꾸로 출력되어 86420이 출력된다.
