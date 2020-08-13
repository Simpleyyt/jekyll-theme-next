---
layout: post
title: "[프로그래머스] K번째 수 _ java"
categories:
  - algorithm
excerpt: " "
comments: true
share: true
tags:
  - java
  - algorithm
  - programmers
  - sort
date: 2020-08-13 T13:19:00-0:05:00
---

# 문제 설명
배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다. <br/>
예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면 <br/>

    1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
    2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
    3. 2에서 나온 배열의 3번째 숫자는 5입니다.

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.<br/><br/>

# 제한사항
array의 길이는 1 이상 100 이하입니다.<br/>
array의 각 원소는 1 이상 100 이하입니다.<br/>
commands의 길이는 1 이상 50 이하입니다.<br/>
commands의 각 원소는 길이가 3입니다.<br/><br/>

# 입출력 예

|array|commands|return|
|-----|------|-----|
|[1, 5, 2, 6, 3, 7, 4] |[[2, 5, 3], [4, 4, 1], [1, 7, 3]]|[5, 6, 3]|

<br/>

# 입출력 예 설명
[1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.<br/>
[1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.<br/>
[1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.<br/><br/>


# 소스코드
```java
import java.util.Arrays;
class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length];
        
        for(int i=0;i<commands.length;i++){
            int[] temp = Arrays.copyOfRange(array, commands[i][0]-1,commands[i][1]);
            Arrays.sort(temp);
            
            answer[i] = temp[commands[i][2]-1];
        }
        return answer;
    }
}
```

Arrays클래스에 대해서는 <https://kimmy100b.github.io/java/2020/08/13/java-arrays/#>여기를 참고해주세요