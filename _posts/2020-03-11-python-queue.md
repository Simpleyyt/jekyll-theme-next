---
title:  "데이터구조 : 큐(Queue)"
excerpt: "큐에 대해 알아보고 파이썬을 사용하여 큐를 코딩하기"
toc: true
toc_sticky: true
toc_label: "Index"

categories:
  - python
tags:
  - python
  - data structure
  - queue
last_modified_at: 2020-03-11T23:54:00-0:05:00
---

## 1. 큐의 구조
- 가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 구조이다.

- 줄을 서는 행위와 유사하다.

  ex) 놀이구기를 탈 때 가장 먼저 줄을 선 사람이 제일 먼저 놀이기구를 타게 된다.

- FIFO(First-In, First-Out) 또는 LILO(Last-In, Last-Out) 방식

  ※ FIFO는 자주 쓰는 용어임으로 알아두기

- 스택과 꺼내는 순서가 반대이다. 스택은 가장 나중에 넣은 데이터가 가장 먼저 꺼낼 수 있는 구조이다.
<br/>
![](https://kimmy100b.github.io/assets/images/python/basic/queue.jpg){: .align-center}<br/>

## 2. 알아둘 용어
queue에는 기능이 두 가지 있다. 하나는 넣는 기능이고 나머지는 빼내는 기능이다. <br/>

- Enqueue : 큐를 데이터를 넣는 기능

- Dequeue : 큐를 데이터를 꺼내는 기능

## 3. 파이썬 queue 라이브러리를 활용하여 큐 자료 구조 구현하기
- queue 라이브러리에는 다양한 큐 구조로 Queue(), LifoQueue(), PriorityQueue() 제공<br/>



|큐 구조| 설명 |
|-----|------|
| Queue() | 가장 일반적인 큐 자료 구조 |
| LifoQueue() | 나중에 입력된 데이터가 먼저 출력되는 구조(스택 구조라고 보면 됨)|
| PriorityQueue() | 데이터마다 우선순위를 넣어서, 우선순위가 높은 순으로 데이터 출력 |





### 3.1. Queue()로 가장 일반적인 큐 만들기(FIFO)
![](https://kimmy100b.github.io/assets/images/python/basic/queue01.jpg){: .align-center}<br/>
queue.Queue에서 queue는 라이브러리이고 Queue는 클래스이다.<br/>
put은 데이터를 넣는 것이다. 넣을 데이터를 괄호 안에 써준다. put(데이터)<br/>
qsize는 큐사이즈를 알려준다.<br/>
get은 데이터를 꺼내는 것이다. <br/>

### 3.2. LifoQueue()로 큐 만들기(LIFO)
![](https://kimmy100b.github.io/assets/images/python/basic/queue02.jpg){: .align-center}<br/>
가장 마지막에 들어간 것이 가장 처음에 나옴으로 처음 get은 가장 마지막에 들어간 1이 출력된다.

### 3.3.PriorityQueue()로 큐 만들기
![](https://kimmy100b.github.io/assets/images/python/basic/queue03.JPG){: .align-center}<br/>
PriorityQueue는 들어간 순서와 상관없이 우선순위에 따라 나오는 데이터가 정해진다. (priority 우선순위)<br/>
우선순위가 높은 것이 숫자가 낮다.<br/> 
그렇기때문에 숫자가 가장 낮은 (5, 1)이 먼저 나온다.<br/>
put((우선순위, 데이터)) 형식으로 데이터를 queue에 넣어준다.<br/>


## 4. 리스트 변수로 큐를 다루는 enqueue, dequeue 작성하기
![](https://kimmy100b.github.io/assets/images/python/basic/queue04.jpg){: .align-center}<br/>


