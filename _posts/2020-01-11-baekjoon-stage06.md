---
title:  "Baekjoon Algorithm stage06_java"
excerpt: "백준알고리즘 6단계 1차원배열"
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
## 10818번_최대최소_step01
![](https://kimmy100b.github.io/assets/images/baekjoon/stage04/step01.jpg)
```java
import java.util.*;

public class Main{
  public static void main(String[] args){
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt();
    int[] arr = new int[n];
    int max=-1000000;
    int min=1000000;
    for(int i=0;i<n;i++)
    {
      arr[i] = sc.nextInt();
      if(max < arr[i])
      {
        max = arr[i];
      }
      if(min > arr[i])
      {
        min = arr[i];
      }
    }
    System.out.println(min+" "+max);
  }
}
```
<br/>

## 2562번_최댓값_step02
![](https://kimmy100b.github.io/assets/images/baekjoon/stage04/step02.jpg)
```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        
        int[]  arr=new int[9];
        int max=0, index=0;
        for(int i=0;i<9;i++)
        {
            arr[i]=sc.nextInt();
             if(arr[i]>max){
                max = arr[i];
                index = i;
            }
        }
        
        System.out.println(max);
        System.out.println(index+1);
      
    }
}
```
<br/>

## 2577번_숫자의 개수_step03
![](https://kimmy100b.github.io/assets/images/baekjoon/stage04/step03-1.jpg)
![](https://kimmy100b.github.io/assets/images/baekjoon/stage04/step03-2.jpg)
```java

```