---
layout: post
title: '<select> 태그의 form 속성'
categories:
    - html
excerpt: ' '
comments: true
share: true
tags:
    - html
    - form
    - select
date: 2020-08-10 T16:34:00-0:05:00
---

# HTML <select> 태그
## 정의
옵션 메뉴를 제공하는 드롭다운 리스트를 정의할 때 사용한다.

## 예제 소스코드
```
<!DOCTYPE html>
<html lang="ko">
<head>
	<meta charset="UTF-8">
	<title>HTML select tag - form attribute</title>
</head>
<body>
    <select name="search" form="myForm">
      	<option value="" selected>-선택-</option>
        <option value="writer">작성자</option>
        <option value="title">제목</option>
        <option value="content">내용</option>
    </select>
</body>
</html>
```

이미지

# <select> 태그의 form 속성
## 정의
<select> 태그의 form 속성은 해당 드롭다운 리스트가 포함될 하나 이상의 <form> 요소를 명시한다.

## 문법
```
<select form="form id">
```

## 예제 소스코드
​```
<!DOCTYPE html>
<html lang="ko">
<head>
	<meta charset="UTF-8">
	<title>HTML select tag - form attribute</title>
</head>
<body>

    <form action="/examples/media/action_target.php" method="get" id="myForm">
        주문자 : <input type="text" name="name"><br>
        <input type="submit"><br><br>
    </form>

    <select name="order" form="myForm">
        <option value="americano">아메리카노</option>
        <option value="caffe latte">카페라테</option>
        <option value="cafe au lait">카페오레</option>
        <option value="espresso">에스프레소</option>
    </select>
    
</body>
</html>
```

이미지

# 참고
http://tcpschool.com/html-tag-attrs/select-form