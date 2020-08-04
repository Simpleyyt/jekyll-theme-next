---
layout: post
title: '백준알고리즘 2단계 if문_java'
categories:
    - algorithm
excerpt: ' '
comments: true
share: true
tags:
    - java
    - algorithm
    - baekjoon
date: 2020-01-08T16:15:00-0:05:00
---

# 1330번\_두 수 비교하기\_step01

![](https://kimmy100b.github.io/assets/images/baekjoon/stage02/step01.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int a = sc.nextInt();
		int b = sc.nextInt();

		if(a>b) {
			System.out.println(">");
		}

		else if(a<b) {
			System.out.println("<");
		}

		else {
			System.out.println("==");
		}
	}
}
```

<br/>

# 9498번\_시험 성적\_step02

![](https://kimmy100b.github.io/assets/images/baekjoon/stage02/step02.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int score = sc.nextInt();

		if(score<=100&&score>=90) {
			System.out.println("A");
		}
		else if(score>=80) {
			System.out.println("B");
		}
		else if(score>=70) {
			System.out.println("C");
		}
		else if(score>=60) {
			System.out.println("D");
		}
		else {
			System.out.println("F");
		}
	}
}

```

<br/>

# 2753번\_윤년\_step03

![](https://kimmy100b.github.io/assets/images/baekjoon/stage02/step03.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int year = sc.nextInt();

		if((year%4==0&&year%100!=0)||year%400==0) {
			System.out.println("1");
		}

		else
		{
			System.out.println("0");
		}
	}
}
```

<br/>

# 2884번\_알람 시계\_step04

![](https://kimmy100b.github.io/assets/images/baekjoon/stage02/step04.jpg)

```java
import java.util.Scanner;

public class step04 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int h = sc.nextInt();//시간
		int m = sc.nextInt();//분


		if(m<45) {
			if(h==0) {
				h=24;
			}
	     	h-=1;
			m+=15;
			System.out.println(h+" "+m);
		}
		else {
			m-=45;
			System.out.println(h+" "+m);
		}
	}
}
```

여기서 조심해야할 부분은 시(hour)가 00이고 분(minute)이 45분이하이면 시가 23시가 되어야한다.
<br/>

# 10817번\_세 수\_step05

![](https://kimmy100b.github.io/assets/images/baekjoon/stage02/step05.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int a=sc.nextInt();
		int b=sc.nextInt();
		int c=sc.nextInt();

		if((b>=a&&a>c)||(c>a&&a>b)) {
			System.out.println(a);
		}
		else if((a>b&&b>=c)||(c>b&&b>a)) {
			System.out.println(b);
		}

		else {
			System.out.println(c);
		}
	}
}
```

<br/>
