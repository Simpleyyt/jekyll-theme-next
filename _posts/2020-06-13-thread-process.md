---
title: "프로세스 vs 스레드"
categories:
  - dictionary
tags:
  - thread
  - process
last_modified_at: 2020-06-13 T00:01:00-0:05:00
---

## 프로그램

- 컴퓨터에서 실행할 수 있는 파일
- 윈도우의 경우, 이름 뒤쪽에 .exe 붙어있는 파일

## 멀티태스킹(Multi-tasking)

- 여러 개의 작업을 동시에 처리
- 다운로드 받으면서 웹 서핑을 하고 카톡을 주고 받는다.

## 프로세스(Process)

- 프로그램이 실행되어서 돌어가고 있는 상태
- 컴퓨터가 어떤 일을 하고 있는 상태
  ![](https://kimmy100b.github.io/assets/images/dictionary/process0.jpg){: .align-center}<br/>
  ![](https://kimmy100b.github.io/assets/images/dictionary/process1.jpg){: .align-center}<br/>

### 특징

- 프로세스는 각각 독립된 메모리 영역(Code, Data, Stack, Heap의 구조)을 할당받는다.
- 기본적으로 프로세스 당 최소 1개의 스레드(메인스레드)를 가진다.
- 각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근 할 수 없다.
- 한 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신을 해야한다.

## 스레드(thread)

- 한 프로세스 내에서 실행되는 흐름의 단위
- 프로세스가 할당받은 자원을 이용하는 실행의 단위
- 한 프로세서도 여러 갈래의 작업들이 동시에 진행될 필요가 있다.
  ![](https://kimmy100b.github.io/assets/images/dictionary/thread.jpg){: .align-center}<br/>

### 특징

- 스레드는 프로세스 내에서 각각 stack만 따로 할당받고 code, data, heap 영역은 공유한다.
  즉, 스레드는 한 프로세스 내에서 동작되는 여러 실행의 흐름으로, 프로세스 내의 주소공간이나 자원들(힙 공간 등)과 같은 프로세스 내에 스레드끼리 공유하면서 실행된다.
- 한 스레드가 프로세스 자원을 변경하면, 다른 이웃 스레드도 그 변경 결과를 즉시 볼 수 있다.
