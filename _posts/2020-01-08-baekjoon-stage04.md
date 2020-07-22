---
title:  "Baekjoon Algorithm stage04_java"
excerpt: "백준알고리즘 4단계 while문"
toc: true
toc_sticky: true
toc_label: "문제 Index"

categories:
  - algorithm
tags:
  - java
  - algorithm
  - baekjoon
last_modified_at: 2020-01-09T15:30:00-0:05:00
---


## 10952번_A+B-5_step01
![](https://kimmy100b.github.io/assets/images/baekjoon/stage04/step01.jpg)
```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner sc =new Scanner(System.in);
        int a = sc.nextInt();
		int b = sc.nextInt();
		
		while(a!=0&&b!=0) {
			System.out.println(a+b);
			
			 a = sc.nextInt();
			 b = sc.nextInt();		
		}
    }
}
```
<br/>

## 10951번_A+B-4_step02
![](https://kimmy100b.github.io/assets/images/baekjoon/stage04/step02.jpg)
```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner s =new Scanner(System.in);
        
        while(s.hasNextInt()){
            int a = s.nextInt();
            int b = s.nextInt();
            
            System.out.println(a+b);
        }
    }
}
```
어떤 조건일 때 종료해야할 지 문제에 제시되어있지 않기 때문에 hasNextInt()를 사용하였다. hasNextInt()는 Int형 이외의 값을 입력받았을 때 처리해주는 함수이다. 그러므로 Int형이 아니면 while문이 종료된다.
<br/>

## 1110번_더하기 사이클_step03
![](https://kimmy100b.github.io/assets/images/baekjoon/stage04/step03-1.jpg)
![](https://kimmy100b.github.io/assets/images/baekjoon/stage04/step03-2.jpg)
```java
import java.util.*;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int n = sc.nextInt();
		int a, b, c;
		int add;
		int count=0;
		
		if(n<0||n>99) {
			System.out.println("N은 0보다 크거나 같고, 99보다 작거나 같은 정수만 입력해야한다.");
			return;
		}
		
		add = n;
		
		while(add!=n||count==0){
			a = add/10; //십의 자리
			b = add%10; //일의 자리
			c = a+b;
			add = b*10+c%10;
			count++;
		}
		System.out.println(count);
	}
}
```