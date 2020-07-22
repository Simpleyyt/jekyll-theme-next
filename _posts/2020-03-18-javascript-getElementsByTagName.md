---
title:  "제어 대상을 찾기 - getElementsByTagName"
excerpt: "태그명을 통해 제어 대상을 찾아보자"
toc: true
toc_sticky: true
toc_label: "Index"

categories:
  - javascript
tags:
  - javascript
  - getElementsByTagName
last_modified_at: 2020-03-18T16:00:00-0:05:00
---

## getElementsByTagName
getElementsByTagName은 인자로 전달된 태그명에 해당하는 객체들을 찾아서 그 리스트를 NodeList라는 유사 배열에 담아서 반환한다. NodeList는 배열은 아니지만 length와 배열접근연산자를 사용해서 element를 조회할 수 있다.

## getElementsByTagName 실습

```
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
<script>
   var lis = document.getElementsByTagName('li');
   for(var i=0;i<lis.length; i++){
       lis[i].style.color="red";
   }
</script>
</body>
</html>
```

![](https://kimmy100b.github.io/assets/images/javascript/getElement/tagName/01-01.jpg)

### <li>태그인데 <ul>아래에 있는 <li>만 바꾸어주고 싶다면??

```
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>

<ol>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ol>

<script>
    var ul = document.getElementsByTagName('ul')[0];
    var lis = ul.getElementsByTagName('li');
    for(var i=0; lis.length; i++){
        lis[i].style.color='red';   
    }
</script>
</body>
</html>
```

![](https://kimmy100b.github.io/assets/images/javascript/getElement/tagName/01-02.jpg)

### 궁금한 점 1. var ul = document.getElementsByTagName('ul')[0];에서 [0]을 없애면?

```
var ul = document.getElementsByTagName('ul');
```

아무것도 변화되지 않음

### 궁금한 점 2. var ul = document.getElementsByTagName('ul')[0];에서 [1]로 바꾸었을 때

```
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>

<ol>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ol>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
<script>
    var ul = document.getElementsByTagName('ul')[1];
    var lis = ul.getElementsByTagName('li');
    for(var i=0; lis.length; i++){
        lis[i].style.color='red';   
    }
</script>
</body>
</html>
```

![](https://kimmy100b.github.io/assets/images/javascript/getElement/tagName/01-03.jpg)