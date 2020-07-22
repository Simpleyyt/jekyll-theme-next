---
title: "백준알고리즘 3단계 for문_java"
categories:
  - algorithm
tags:
  - java
  - algorithm
  - baekjoon
last_modified_at: 2020-01-08T23:26:00-0:05:00
---

## 2739번\_구구단\_step01

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step01.jpg)

```java
import java.util.Scanner;

public class Main{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int n=sc.nextInt();

		for(int i=1;i<=9;i++) {
			System.out.println(n+" * "+i+" = "+(n*i));
		}
	}
}
```

<br/>

## 10950번\_A+B-3_step02

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step02.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int t=sc.nextInt();//test case의 개수

		for(int i=0;i<t;i++) {
			int a = sc.nextInt();
			int b = sc.nextInt();
			System.out.println(a+b);
		}
	}
}
```

<br/>

## 8393번\_합\_step03

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step03.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int n = sc.nextInt();
		int sum=0;//합계

		for(int i=1;i<=n;i++) {
			sum+=i;
		}

		System.out.println(sum);
	}
}
```

<br/>

## 15552번\_빠른 A+B_step04

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step04.jpg)
![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step04-Buffered.jpg)
BufferedReader/BufferedWrite는 버퍼를 이용해서 읽고 쓰는 함수이다. 버퍼를 사용하기 때문에 입출력의 효율성이 좋다. 예를 들어 흙을 파서 언덕으로 옮길 경우, 한 번 삽질할 때마다 가서 옮기는 것보다 수레에 가득 채워서 한번에 나르는 것이 효율적인 것과 같은 이치이다. 그러므로 Scanner를 사용하는 것보다 BufferedReader(입력)/BufferedWrite(출력)를 사용하는 것이 입출력할 때 더 빠르고 효율적이다.

```java
import java.io.*;

public class Main {
	public static void main(String[] args) {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

		try {
			int n = Integer.parseInt(br.readLine());
			if(n>1000000) {
				System.out.println("입력값이 1,000,000 넘었습니다.");
                return;
			}
			for(int i=0;i<n;i++)
			{
				String[] word = br.readLine().split(" ");
				int a = Integer.parseInt(word[0]);
				int b = Integer.parseInt(word[1]);
				if(a<1||a>1000||b<1||b>1000) {
					System.out.print("A 또는 B는 1 이상, 1,000 이하가 아닙니다.");
					return;
				}
				 bw.write((a+b)+"\n");
			}
		bw.flush();
		bw.close();

		}catch(IOException e){

		}
	}
}
```

이 문제를 해결할 때 막혔던 부분은 splite함수이다. splite 함수는 간단하게 말하자면 특정 문자를 기준으로 문자열을 잘라 배열에 넣어주는 함수이다. splite 함수와 그 외에 유사한 함수들은 포스팅을 따로 올릴 예정이다.
<br/>

## 2741번\_N찍기\_step05

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step05.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int n = sc.nextInt();

		for(int i=1;i<=n;i++)
		{
			System.out.println(i);
		}
	}
}
```

<br/>

## 2742번\_기찍N_step06

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step06.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int n = sc.nextInt();

		for(;n>=1;n--) {
			System.out.println(n);
		}
	}
}
```

<br/>

## 11021_A+B-7_step07

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step07.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int t = sc.nextInt();
		int a,b; //계산하기 위한 변수

		for(int i=1;i<=t;i++) {
			a = sc.nextInt();
			b = sc.nextInt();

			System.out.println("Case #"+i+": "+(a+b));
		}
	}
}
```

<br/>

## 11022_A+B-8_step08

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step08.jpg)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int t = sc.nextInt();
		int a,b;

		for(int i=1;i<=t;i++) {
			a = sc.nextInt();
			b = sc.nextInt();

			System.out.println("Case #"+i+": "+a+" + "+b+" = "+(a+b));
		}
	}
}
```

<br/>

## 2438번\_별 찍기1_step09

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step09.jpg)

```java
import java.util.*;

public class Main {
    public static void main(String[] args){

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

		for(int i=1;i<=n;i++)
		{
			for(int j=1;j<=i;j++)
			{
				System.out.print("*");
			}
			System.out.println();
		}
	}
}
```

<br/>

## 2439번\_별 찍기2_step10

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step10.jpg)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args){

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

		for(int i=1;i<=n;i++)
		{
			for(int j=n;j>i;j--)
			{
				System.out.print(" ");
			}
			for(int j=1;j<=i;j++)
			{
				System.out.print("*");
			}
			System.out.println();
		}
	}
}
```

<br/>

## 10871번\_X보다 작은 수\_step11

![](https://kimmy100b.github.io/assets/images/baekjoon/stage03/step11.jpg)

```java
import java.util.*;

public class Main{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int n = sc.nextInt();
		int x = sc.nextInt();
		int[] a = new int[n];

		for(int i=0;i<n;i++)
		{
			a[i] = sc.nextInt();
		}

		for(int i=0;i<a.length;i++)
		{
			if(a[i]<x) {
				System.out.print(a[i]+" ");
			}
		}
	}
}
```
