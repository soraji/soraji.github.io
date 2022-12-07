---
layout: post
title:  "[Vue.js] Vue 자식컴포넌트를 부모컴포넌트 위로 올리기(레이어처럼)"
categories: front 
comments: true
---



자식 컴포넌트가 부모 컴포넌트 위로 올라오게 해서 보이게 하려면(레이어 인것처럼)

부모body css에

```css
overflow:visible;
```

넣어주면 된다.

