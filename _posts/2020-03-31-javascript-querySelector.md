---
title:  "제어 대상을 찾기 - querySelector, querySelectorAll"
excerpt: "querySelector과 querySelectorAll에 대해 알아보자"
toc: true
toc_sticky: true
toc_label: "Index"

categories:
  - javascript
tags:
  - javascript
  - querySelector
  - querySelectorAll
last_modified_at: 2020-03-31T13:00:00-0:05:00
---

## querySelector
CSS는 선택자라는 것이 있어서 그 선택자를 이용해서 여러분이 꾸며주고자하는 element를 디자인할 수 있는 기능이 있다.<br/>
그 선택자를 인자로 받아서 그 선택자에 해당되는 element들의 객체를 찾아서 여러분들에게 리턴해주는 메소드가 querySelector라고 하는 것이다.<br/>
그러나 단 한 개만 선택하여 꾸며준다. 만약, 다수의 태그를 사용하고 싶다면 querySelectorAll을 사용해야한다.(뒤에 나옴)<br/>

## querySelector 실습
코드

```
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li id="activeI">JavaScript</li>
</ul>
<ol>
    <li>HTML</li>
    <li class="activeC">CSS</li>
    <li>JavaScript</li>
</ol>
 
<script>
    var li = document.querySelector('li');
    li.style.color='red';
    var li = document.querySelector('.activeC');
    li.style.color='blue';
    var li = document.querySelector('#activeI');
    li.style.color='green';
</script>
</body>
</html>
```

코드 설명
1. <li>태그를 의미한다.

```
document.querySelector('li');
```

2. .이 들어가면 classname을 의미한다.

```
document.querySelector('.activiteC');
```

3. #이 들어가면 id를 의미한다.

```
document.querySelector('#activiteI');
```

결과


### 궁금한 점 - 1
class가 여러 개라면 어떻게 적용될까?

```
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li id="activeI">JavaScript</li>
</ul>
<ol>
    <li>HTML</li>
    <li class="activeC">CSS</li>
    <li class="activeC">JavaScript</li>
</ol>
 
<script>
    var li = document.querySelector('li');
    li.style.color='red';
    var li = document.querySelector('.activeC');
    li.style.color='blue';
    var li = document.querySelector('#activeI');
    li.style.color='green';
</script>
</body>
</html>
```

결론 : className을 더 추가해줘도 전부 스타일을 꾸며주는 것이 아니라 해당하는 className의 제일 처음만 꾸며준다.

## querySelectorAll
querySelector과 기본적인 동작방법은 같지만 모든 객체를 조회한다는 점이 다르다.

## querySelectorAll 실습
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
    <li class="active">CSS</li>
    <li>JavaScript</li>
</ol>
 
<script>
    var lis = document.querySelectorAll('li');
    for(var name in lis){
        lis[name].style.color = 'blue';
    }
</script>
</body>
</html>
```

### 궁금한 점 - 1
getElementsByTagName처럼 <ul>태그 안에 있는 <li>만 선택할 수 있을까?<br/>

첫번째 방법(실패)
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
    <li class="active">CSS</li>
    <li>JavaScript</li>
</ol>
 
<script>
    var uls = document.querySelectorAll('ul');
    var lis = uls.querySelectorAll('li');
    for(var name in lis){
        lis[name].style.color = 'red';
    }
</script>
</body>
</html>
```
