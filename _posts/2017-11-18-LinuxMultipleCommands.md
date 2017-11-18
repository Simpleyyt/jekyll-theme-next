---
title: Linux 다중명령어 실행
date: 2017-11-18 10:09:00
categories: MISC
tags: MISC, Linux
---

## 리눅스 다중 명령어 전달

리눅스 쉘에서는 한번에 여러가지 명령어를 전달할 수 있다.

&&, \|\|, ; 이런 옵션들을 명령어와 명령어 사이에 붙여서 여러개의 명령어를 전달할 수 있다.

#### ; - 무조건 다음 명령어 실행

; 은 앞의 명령어 실행결과, 즉 성공여부와 상관없이 다음 명령어를 실행한다. 

#### \|\| - 앞의 명령어가 실패시 다음 명령어 실행

\|\| 는 앞의 명령어의 리턴 값이 0이 아닐때 다음 명령어를 실행하도록 되어 있다.
참고로 명령이 성공적이라면 리턴값은 0이 나온다.

; 과  \|\|의 차이는 아래의 실행결과를 보면 알 수 있다.

    user1@ubuntu:~/temp$ ls -l
    total 0
    user1@ubuntu:~/temp$ mkdir tmp1;rmdir tmp1
    user1@ubuntu:~/temp$ ls
    user1@ubuntu:~/temp$ mkdir tmp1||rmdir tmp1
    user1@ubuntu:~/temp$ ls
    tmp1
    user1@ubuntu:~/temp$

#### && - 첫번째 명령이 성공적일 시에만 다음 명령어 실행

&&는 \|\|와 정반대로 앞의 명령어가 성공적으로 완료되었을 경우에만 다음 명령어로 넘어가게 한다.
리턴값이 0일 경우에만 넘어가는 것이다.

## 리눅스 명령어 리다이렉션

#### 표준 입출력 리다이렉션
입출력 리다이렉션이란 실행결과를 쉘로 출력하는 대신 바로 다른곳으로 넘기는 것이다.

|>	|표준 출력|ls > note.txt|ls의 결과를 note.txt에 저장|
|>>	|표준 출력 추가|ls >> note.txt|ls의 결과를 note.txt의 끝부분에 추가하여 저장|
|<	|표준 입력|user1@ubuntu:~/temp/tmp1$ grep Evon < note.txt |note.txt를 읽어들여 grep으로 Evon이 들어간 줄을 검색|

아래는 표준 입력을 사용한 예시이다.

    user1@ubuntu:~/temp/tmp1$ cat note.txt 
    Jerry Campagna  
    Carylon Toth  
    Carolin Curtsinger  
    Irving Bill  
    Evon Larger  
    Lacy Priddy  
    Hilaria Wiesen  
    Melani Barefield  
    user1@ubuntu:~/temp/tmp1$ grep Evon < note.txt 
    Evon Larger 


#### \| - 앞의 명령어 실행결과를 다음 명령어에서도 사용
싱글 파이프는 앞에서 실행된 출력결과를 다음 명렁어로 넘겨주는데에 사용된다.
바로 위에서 사용된 예제는 아래처럼 바꿀 수도 있다.

    user1@ubuntu:~/temp/tmp1$ cat note.txt | grep Evon
    Evon Larger

#### 파일 디스크립터

0	|	표준입력(키보드)
1	|	표준출력(모니터)
2	|	표준에러(모니터)

## 기타 명령어 옵션

#### & - 백그라운드 실행

엠퍼센드 하나만 명령어의 끝에 붙이면 이를 백그라운드 작업으로 실행한다는 의미이다.

    user1@ubuntu:~/Desktop$ python3 script.py&
    [1] 12552
    user1@ubuntu:~/Desktop$

실행시 위처럼 PID 값을 준 후 쉘로 돌아간다. 작업 종료시에는 `kill 12552` 로 종료시킬 수 있다.

#### !# - 동일한 명령어를 2번 실행

사실 이 명령어의 필요성은 잘 못느끼겠으나 신기해서 함께 올린다.
아래의 코드를 보고 이해하자.

    user1@ubuntu:~/temp$ ls
    tmp1
    user1@ubuntu:~/temp$ ls;!#
    ls;ls;
    tmp1
    tmp1
    user1@ubuntu:~/temp$ 

이처럼 `ls;!#`은 바로 아래에서 `ls;ls;` 라고 바뀌어서 실행된다고 알려준 뒤 `ls`명령을 2번 연달아 실행한 결과를 출력해주었다.
