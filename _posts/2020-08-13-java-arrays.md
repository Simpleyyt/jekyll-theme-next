---
layout: post
title: "[JAVA] Arrays 클래스"
categories:
  - java
excerpt: " "
comments: true
share: true
tags:
  - java
  - Arrays
  - class
date: 2020-08-13 T14:19:00-0:05:00
---

# java.util.Array 클래스
Arrays 클래스에는 배열을 다루기 위한 다양한 메소드가 포함되어 있다.<br/><br/>

# binarySearch() 메소드
전달받은 배열에서 특정 객체를 이진 검색 알고리즘을 사용하여 검색한 후, 그 위치를 반환함<br/>
```java
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
		int[] arr = new int[1000];
		for(int i = 0; i < arr.length; i++) {
			arr[i] = i;
		}
		
		System.out.println(Arrays.binarySearch(arr, 437));
	}
}
```
<br/>출력결과
```
437
```
<br/>

# copyOf() 메소드
전달받은 배열을 특정 길이의 새로운 배열로 복사하여 반환함<br/>
`Arrays.copyOf(배열명, 길이)`<br/>
주어진 길이만큼 배열을 복사하여 반환한다.
```java
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
		int[] arr1 = {1, 2, 3, 4, 5};
		int[] arr2 = Arrays.copyOf(arr1, 3);
		
		for (int i = 0; i < arr2.length; i++) {
			System.out.print(arr2[i] + " ");
		}
		System.out.println();
		
		int[] arr3 = Arrays.copyOf(arr1, 10);
		for (int i = 0; i < arr3.length; i++) {
			System.out.print(arr3[i] + " ");
		}
	}
}
``` 
<br/> 출력결과
```
1 2 3 
1 2 3 4 5 0 0 0 0 0
```
<br/>

# copyOfRange() 메소드
전달받은 배열의 특정 범위에 해당하는 요소만을 새로운 배열로 복사하여 반환함<br/>
`Arrays.copyOfRange(배열명, 시작위치, 마지막위치)` <br/>
배열의 시작위치에서 마지막위치까지(마지막위치는 포함안됨) 해당하는 요소만을 가지고 새 배열로 복사하여 반환한다.<br/>
```java
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
    int[] arr1 = {1, 2, 3, 4, 5};
    int[] arr2 = Arrays.copyOfRange(arr1, 2, 4);
    for (int i = 0; i < arr2.length; i++) {
      System.out.print(arr2[i] + " ");
    }
	}
}
```
<br/>출력결과
```
3 4
```
<br/>
만약, 배열의 길이보다 마지막 위치의 숫자가 더 길 경우

```java
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
    int[] arr1 = {1, 2, 3, 4, 5};
    int[] arr2 = Arrays.copyOfRange(arr1, 2, 7);
    for (int i = 0; i < arr2.length; i++) {
      System.out.print(arr2[i] + " ");
    }
	}
}
```
<br/>출력결과
```
3 4 5 0 0 
```
<br/>

# equals() 메소드
전달받은 두 배열이 같은지를 확인함. boolean 타입<br/>
```java
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
    int[] arrA = {1,2,3};
    int[] arrB = {1,3,4};
    int[] arrC = {1,2,3}; 
    
    System.out.println(Arrays.equals(arrA, arrB));
    System.out.println(Arrays.equals(arrA, arrC));
	}
}
```
<br/>출력결과
```
false
true
```
<br/>

# fill() 메소드
전달받은 배열의 모든 요소를 특정 값으로 초기화함<br/>
`Arrays.fill(배열명, 넣을 값)`<br/>
배열의 모든 값을 넣을 값으로 바꾸어줌<br/>

```java
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
        int[] arr = new int[10];
    
        Arrays.fill(arr, 3);
        for (int i = 0; i < arr.length; i++) {
        System.out.print(arr[i] + " ");
        }
	}
}
```
<br/>출력결과

```
3 3 3 3 3 3 3 3 3 3
```
<br/>

# sort() 메소드
전달받은 배열의 모든 요소를 오름차순으로 정렬함
```java
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
		int[] arr = {5, 3, 4, 1, 2};
		
		Arrays.sort(arr);
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
	}
}
```
<br/>
출력결과

```
1 2 3 4 5
```
