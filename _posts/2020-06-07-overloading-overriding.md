---
title: "오버로딩 vs 오버라이딩"
categories:
  - java
tags:
  - java
  - overloading
  - overriding
last_modified_at: 2020-06-07 T23:41:00-0:05:00
---

## 오버로딩(Overloading)

오버로딩 = 메서드 오버로딩<br/>
: 한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것<br/>

오버로딩의 조건<br/>

1. 메서드 이름이 같아야 한다.
2. 매개변수의 개수 또는 타입이 달라야한다.

## 오버로딩의 예

PrintStream 클래스 내 println 메서드

```
void println()
void println(boolean x)
void println(char x)
void println(char[] x)
void println(double x)
void println(float x)
void println(int x)
void println(long x)
void println(String x)
```

add 메서드

```
long add(int a, long b) { return a+b; }
long add(long a, int b) { return a+b; }
```

## 오버로딩의 장점

1. 메서드의 이름을 짓기 편리하고 쉽게 기억하고 예측할 수 있어 오류의 가능성을 줄일 수 있다.
2. 메서드의 이름을 절약할 수 있다.

## 오버라이딩(Overriding)

부모 클래스로부터 메소드를 상속받았으나 그 메소드를 자식클래스에서 재사용하는 것이다.

## 오버라이딩의 예

```
class Dog{
    void sound() {
        System.out.println("멍멍");
    }
}


class Poodle extends Dog{
    @Override
    void sound() {
        System.out.println("왈왈(say 푸들)");
    }
}

class Pug extends Dog{
    @Override
    void sound() {
        System.out.println("뭉뭉(say 퍼그)");
    }
}


class Chihuahua extends Dog{
    @Override
    void sound() {
        System.out.println("월월(say 치와와)");
    }
}
```

![](https://kimmy100b.github.io/assets/images/java/overriding.png){: .align-center}<br/>

## 오버로딩과 오버라이딩의 차이

| 구분           | 오버로딩 | 오버라이딩 |
| -------------- | -------- | ---------- |
| 메서드 이름    | 같음     | 같음       |
| 매개변수, 타입 | 다름     | 같음       |
| 리턴타입       | 상관없음 | 같음       |
