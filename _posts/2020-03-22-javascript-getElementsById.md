---
layout: post
title: '제어 대상을 찾기 - getElementsById'
categories:
    - javascript
excerpt: ' '
comments: true
share: true
tags:
    - javascript
    - getElementsById
date: 2020-03-22T15:00:00-0:05:00
---

# getElementsById

id 값을 기준으로 객체를 조회한다. <br/>
성능면에서 가장 우수하다.<br/>
id 값으로 객체는 한 개밖에 조회하지 못한다.<br/>
왜냐하면 html에서 id는 한개만 있기 때문이다. id는 중복할 수 없다.<br/>

# getElementsById 실습

```
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li id="active">CSS</li>
    <li>JavaScript</li>
</ul>
<script>
    var li = document.getElementById('active');
    li.style.color='red';
</script>
</body>
</html>
```

![](https://kimmy100b.github.io/assets/images/javascript/getElement/className/01-01.jpg)
