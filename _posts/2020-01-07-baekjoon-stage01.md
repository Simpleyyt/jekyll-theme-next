---
title: "백준알고리즘 1단계 입출력과 사칙연산_java"
categories:
  - algorithm
tags:
  - java
  - algorithm
  - baekjoon
last_modified_at: 2020-01-07T23:22:00-0:05:00
---

## 2557번\_Hello World_step01

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step01.jpg)

```java
public class Main{
    public static void main(String[] args){
        System.out.println("Hello World!");
    }
}
```

<br/>

## 10718번\_We love kriii_step02

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step02.jpg)

```java
public class Main {
    public static void main(String args[]){
        System.out.println("강한친구 대한육군");
        System.out.println("강한친구 대한육군");
    }
}
```

<br/>

## 10171번\_고양이\_step03

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step03.jpg)

```java
public class Main{
    public static void main(String[] args){
        System.out.println("\\    /\\");
        System.out.println(" )  ( ')");
        System.out.println("(  /  )");
        System.out.println(" \\(__)|");
    }
}
```

<br/>

## 10172번\_개\_step04

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step04.jpg)

```java
public class Main{
    public static void main(String[] args){
        System.out.println("|\\_/|");
        System.out.println("|q p|   /}");
        System.out.println("( 0 )\"\"\"\\");
        System.out.println("|\"^\"`    |");
        System.out.println("||_/=\\\\__|");
    }
}
```

<br/>

## 1000번\_A+B_step05

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step05.jpg)

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);

        int x,y;
        x = sc.nextInt();
        y = sc.nextInt();

        System.out.printf("%d",x+y);
    }
}
```

<br/>

## 1001번\_A-B_step06

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step06.jpg)

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);

        int a = sc.nextInt();
        int b = sc.nextInt();

        System.out.print(a-b);
    }
}
```

<br/>

## 10998번\_A\*B_step07

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step07.jpg)

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);

        int x = sc.nextInt();
        int y = sc.nextInt();

        System.out.print(x*y);
    }
}
```

<br/>

## 1008번\_A/B_step08

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step08.jpg)

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);

        int x = sc.nextInt();
        int y = sc.nextInt();
        double result = (double)x/y;

        System.out.print(result);
    }
}
```

<br/>

## 10869번\_사칙연산\_step09

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step09.jpg)

```java
import java.util.Scanner;

public class Main {
	    public static void main(String[] args){
	        Scanner sc = new Scanner(System.in);

	        int a = sc.nextInt();
	        int b = sc.nextInt();

	        System.out.println(a+b);
	        System.out.println(a-b);
	        System.out.println(a*b);
	        System.out.println(a/b);
	        System.out.println(a%b);

	}
}
```

<br/>

## 10430번\_나머지\_step10

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step10.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int a = sc.nextInt();
		int b = sc.nextInt();
		int c = sc.nextInt();

		System.out.println((a+b)%c);
		System.out.println((a%c+b%c)%c);
		System.out.println((a*b)%c);
		System.out.println((a%c*b%c)%c);
	}
}
```

<br/>

## 2588번\_곱셈\_step11

![](https://kimmy100b.github.io/assets/images/baekjoon/stage01/step11.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc=new Scanner(System.in);

		int a = sc.nextInt();
		int b = sc.nextInt();

		System.out.println(a*(b%10));
		int tmp = b/10;
		System.out.println(a*(tmp%10));
		tmp=tmp/10;
		System.out.println(a*(tmp%10));
		System.out.println(a*b);
	}
}
```
