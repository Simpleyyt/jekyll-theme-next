---
layout: post
title: '제어 대상을 찾기 - getElementsByClassName'
categories:
    - javascript
excerpt: ' '
comments: true
share: true
tags:
    - javascript
    - getElementsByClassName
date: 2020-03-22T11:00:00-0:05:00
---

# getElementsByClassName

class 속성의 값을 기준으로 객체를 조회할수도 있다. <br/>
자바스크립트에서는 class를 class라고 사용하지 않고 classname이라고 사용한다. <br/>
왜냐하면 자바에서의 class와 다른 개념이기 때문이다. <br/>

# getElementsByClassName 실습

```
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li class="active">CSS</li>
    <li class="active">JavaScript</li>
</ul>
<script>
    var lis = document.getElementsByClassName('active');
    for(var i=0;i<lis.length;i++){
        lis[i].style.color='red';
    }
</script>
</body>
</html>
```

![](https://kimmy100b.github.io/assets/images/javascript/getElement/className/01-01.jpg)
