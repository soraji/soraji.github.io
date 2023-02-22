---
layout: post
title: "[ Java ] 날짜 비교하기"
categories: back
comments: true
---

기준날짜로부터 전인지 후인지 비교하기

```java
SimpleDateFormat sdfYEAR = new SimpleDateFormat("yyyy");
SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");

Calendar cal = Calendar.getInstance();
cal.add(cal.YEAR, -1);
Date pyear = cal.getTime();

Date today = new Date();

String pyeonggadate = "20190729";
Date eyear = sdf.parse(pyeonggadate);

int compare = sdf.format(today).compareTo(sdf.format(ekffayear));
if(compare > 0){
    System.out.println("오늘은 2019년 07월 29일 이후입니다.");
cal.add(cal.YEAR, 1);
pyear = cal.getTime();
}
```

sdfYEAR은 sdf을 이용해서 년도만 뽑아오는 형식으로 만들었다.

sdf는 년월일 형식으로 뽑아왔다.

그때,

```java
String pyeonggadate = "20190729";
Date eyear = sdf.parse(pyeonggadate);
```

String을 Date형식으로 바꾸어서 20190729를 기준날짜로 잡았고,

오늘날짜를 sdf형식으로 ex)20190415 라고 잡아두었다.

그리고 compareTo메서드를 사용하여 비교값이 1인지, 0인지, -1인지 판단한다.

```
A.compareTo(B) 에서

A = B    => 0
A < B    => -1
A > B    => 1
```

---

```
int compare = sdf.format(today).compareTo(sdf.format(eyear));
```

today가 eyear보다 이전이라면 (7/29일 이전) compare은 -1이 될것이고, 7/29일 이후라면 1이 될것이다.
