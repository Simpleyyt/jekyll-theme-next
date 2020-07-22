---
title: "데이터구조 : 스택(Stack) - python"
categories:
  - python
tags:
  - python
  - data structure
  - stack
last_modified_at: 2020-03-12T17:27:00-0:05:00
---

## 1. 스택 구조

- 스택은 LIFO(Last-In, First-Out) 또는 FILO(Fist-In, Last-Out) 데이터 관리 방식을 따른다.

- 한쪽 끝에서만 데이터를 넣거나 뺄 수 있는 구조이다.

- 대표적인 스택의 활용 : 컴퓨터 내부의 프로세스 구조의 함수 동작방식

  ex) 웹 브라우저 뒤로 가기를 눌렀을 때 가장 마지막에 들어간 페이지가 나온다.

- 주요기능

  ▶ push() : 데이터를 스택에 넣기<br/>
  ▶ pop() : 데이터를 스택에서 꺼내기<br/>
  ![](https://kimmy100b.github.io/assets/images/python/basic/stack.jpg){: .align-center}<br/>

## 2. 스택 구조와 프로세스 스택

- 스택 구조는 프로세스 실행 구조의 가장 기본
  ![](https://kimmy100b.github.io/assets/images/python/basic/stack01.jpg){: .align-center}<br/>

## 3. 자료 구조 스택의 장단점

▶ 장점<br/>

- 구조가 단순해서 구현이 쉽다.
- 데이터 저장/읽기 속도가 빠르다.<br/>

▶ 단점 (일반적인 스택 구현 시)<br/>

- 데이터 최대 갯수를 미리 정해야 한다. (파이썬의 경우, 재귀 함수는 1000번까지 호출가능)
- 저장 공간의 낭비가 발생할 수 있다.

## 4. 파이썬 리스트 기능에서 제공하는 메서드

- append(push), pop 메서드 제공
  ![](https://kimmy100b.github.io/assets/images/python/basic/stack02.jpg){: .align-center}<br/>
  (data_stack으로 스택 안에 있는 데이터를 모두 출력할 수 있다.)

## 5. 프로그래밍 연습

리스트 변수로 스택을 다루는 pop, push 기능 구현해보기<br/>
(pop, push 함수 사용하지 않고 직접 구현해보기)
![](https://kimmy100b.github.io/assets/images/python/basic/stack04.jpg){: .align-center}<br/>
​
