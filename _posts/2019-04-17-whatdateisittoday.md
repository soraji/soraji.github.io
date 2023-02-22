---
layout: post
title: "[ Java ] 오늘의 요일구하기"
categories: back
comments: true
---

날짜포맷으로 날짜를 가져는 왔는데 이번에는 요일을 구하고싶다면?

```java
String[] weekDay = { "일요일", "월요일", "화요일", "수요일", "목요일", "금요일", "토요일" };
Calendar cal2 = Calendar.getInstance();
int num = cal2.get(Calendar.DAY_OF_WEEK)-1;
String whatdate = weekDay[num];

System.out.println(num);
System.out.println("오늘의 요일 : " + today );
```
