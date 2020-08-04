---
layout: post
title: '카멜(Camel)표기법, 팟홀(Pothole)표기법, 파스칼(Pascal)표기법, 케밥(Kebab)표기법'
categories:
    - knowledge
excerpt: ' '
comments: true
share: true
tags:
    - Camel Case
    - Pothole Case
    - Pascal Case
    - Snack Case
    - Kebab Case
date: 2020-08-04T17:31:00-0:05:00
---

# 카멜표기법(Camel case)

- 낙타의 등모습과 유사
- 변수명은 모두 소문자로 작성하지만 여러 단어가 이어지는 경우, 첫단어를 제외하고 각 단어의 첫글자만 대문자로 지정해 주는 방식
```
int myFirstVariable;
```

# 파스칼표기법(Pascal case)

- 낙타의 등모습과 유사
- 변수명은 모두 소문자로 작성하지만 여러 단어가 이어지는 경우, 각 단어의 첫글자만 대문자로 지정해 주는 방식 (첫 단어의 첫번째 글자도 대문자로 선언)
```
int MyFirstVariable;
```
​
# 팟홀표기법(Pothole case) 또는 스네이크 표기법(Snack Case)

- 뱀처럼 길게 생김
- 모든 단어가 소문자로 시작하고, 단어와 단어 사이에는 "_"로 연결된 방식
```
int my_first_variable;
```
​
# 케밥 표기법(Kebab case)

- 케밥이 꼬챙이에 꽃힌 모습에서 생긴 방법
- 모든 단어가 소문자로 시작하고, 단어와 단어 사이에는 "-"로 연결된 방식
```
int my-first-variable;
```