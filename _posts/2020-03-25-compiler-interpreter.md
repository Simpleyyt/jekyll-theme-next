---
layout: post
title: '컴파일러(Compiler)와 인터프리터(Interpreter)'
categories:
    - knowledge
excerpt: ' '
comments: true
share: true
tags:
    - Compiler
    - Interpreter
    - Cross-Compiler
date: 2020-03-25T01:58:00-0:05:00
---

프로그램을 해석하는 방법 중 대표적인 컴파일러와 인터프리터에 대해 알아보자!<br/>
더 나아가 크로스 컴파일러가 무엇인지도 살펴보자<br/>

## 컴파일러(Compiler)

컴파일러는 고급언어로 쓰인 프로그램을 컴퓨터에서 즉시 실행될 수 있는 형태의 목적 프로그램으로 바꾸어 주는 번역 프로그램이다.<br/>
즉, 고급언어로 쓰인 프로그램을 저급언어로 쓰인 프로그램으로 바꾸어 주는 번역 프로그램이다.<br/>
여기서 오해하면 안되는 부분은 컴퍼일러가 고급언어를 저급언어로 바꾸는 번역 프로그램은 아니다는 것이다.<br/>
![](https://kimmy100b.github.io/assets/images/knowledge/compiler.jpg){: .align-center}<br/>

## 크로스 컴파일(Cross-Compiler)

크로스 컴파일러는 소스 프로그램을 다른 기종에 대한 기계어로 번역하는 컴파일러이다.<br/>
그렇기 때문에 컴파일러를 하는 기종과 컴파일된 프로그램을 수행하는 기종은 서로 다르다.<br/>
예를 들어 스마트폰은 용량이 좋지 않기에 컴파일러를 하기에 좋은 조건이 아니다.<br/>
그래서 컴퓨터에서 스마트폰에서 작동되는 기계어로 컴파일한 다음 그 기계어로 된 프로그램을 스마트폰에 들고 오는 것을 크로스 컴파일러라고 한다.<br/>
![](https://kimmy100b.github.io/assets/images/knowledge/crosscompiler.jpg){: .align-center}<br/>

## 인터프리터(Interpreter)

인터프리터는 고급언어로 작성된 코드를 한 단계씩 해석하여 실행시키는 방법을 말한다.<br/>
그렇기 때문에 소스 프로그램을 목적 프로그램으로 생성하지 않고 직접 수행해서 그 결과를 출력해주는 프로그램이다.<br/>
예를 들어 파이썬이나 LISP 같은 언어들은 인터프리터에서 실행된다.<br/>
![](https://kimmy100b.github.io/assets/images/knowledge/interpreter.jpg){: .align-center}<br/>

## 컴파일러와 인터프리터의 차이

![](https://kimmy100b.github.io/assets/images/knowledge/compilerAndInterpreter.jpg){: .align-center}<br/>
