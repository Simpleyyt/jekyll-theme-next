---
layout: post
title: '[프로그래머스] 없어진 기록 찾기'
categories:
    - query
excerpt: ' '
comments: true
share: true
tags:
    - MySQL
    - Oracle
    - programmers
date: 2020-08-06 T13:34:00-0:05:00
---

# 문제 설명

ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

![](https://kimmy100b.github.io/assets/images/programmers/59042/1.PNG){: .align-center}

ANIMAL_OUTS 테이블은 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블입니다. ANIMAL_OUTS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, NAME, SEX_UPON_OUTCOME는 각각 동물의 아이디, 생물 종, 입양일, 이름, 성별 및 중성화 여부를 나타냅니다. ANIMAL_OUTS 테이블의 ANIMAL_ID는 ANIMAL_INS의 ANIMAL_ID의 외래 키입니다.


![](https://kimmy100b.github.io/assets/images/programmers/59042/2.PNG){: .align-center}

천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

예시

예를 들어, ANIMAL_INS 테이블과 ANIMAL_OUTS 테이블이 다음과 같다면


![](https://kimmy100b.github.io/assets/images/programmers/59042/3.PNG){: .align-center}

ANIMAL_OUTS 테이블에서

Allie의 ID는 ANIMAL_INS에 없으므로, Allie의 데이터는 유실되었습니다.

Gia의 ID는 ANIMAL_INS에 있으므로, Gia의 데이터는 유실되지 않았습니다.

Spice의 ID는 ANIMAL_INS에 없으므로, Spice의 데이터는 유실되었습니다.

따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

![](https://kimmy100b.github.io/assets/images/programmers/59042/4.PNG){: .align-center}

# 소스코드
## 오라클

오라클은 MINUS를 이용하여 join 없이 쉽게

```
SELECT ANIMAL_ID, NAME
FROM ANIMAL_OUTS
MINUS
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
```
## MySQL

```
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_INS AS INS RIGHT JOIN ANIMAL_OUTS AS OUTS 
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.ANIMAL_ID IS NULL;
```

# 링크

https://programmers.co.kr/learn/courses/30/lessons/59042

