---
layout: post
title: "[ Python ] 파이썬 pandas DataFrame column추가/ 데이터프레임 컬럼추가"
categories: back
comments: true
---

pandas를 이용해서 데이터프레임을 만드는데 만약 컬럼을 하나 더 만들고싶다면

```python
df["star"] = pd.DataFrame({"star":star_list})
```

을 넣으면 star로 된 이름의 컬럼이 옆에 생긴다.

```python
df = pd.DataFrame({"title":name_list})
df["star"] = pd.DataFrame({"star":star_list})
df.tail()
```

위 코드는 title이라는 컬럼과 star라는 컬럼이 생기고 row로는 name_list 값과 star_list값이 있는 표가 생긴다.
