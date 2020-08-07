---
layout: post
title: '[프로그래머스] 있었는데요 없었습니다'
categories:
    - query
excerpt: ' '
comments: true
share: true
tags:
    - MySQL
    - Oracle
    - programmers
date: 2020-08-07 T17:34:00-0:05:00
---

# 문제설명

ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

이미지

ANIMAL_OUTS 테이블은 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블입니다. ANIMAL_OUTS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, NAME, SEX_UPON_OUTCOME는 각각 동물의 아이디, 생물 종, 입양일, 이름, 성별 및 중성화 여부를 나타냅니다. ANIMAL_OUTS 테이블의 ANIMAL_ID는 ANIMAL_INS의 ANIMAL_ID의 외래 키입니다.

이미지

관리자의 실수로 일부 동물의 입양일이 잘못 입력되었습니다. 보호 시작일보다 입양일이 더 빠른 동물의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일이 빠른 순으로 조회해야합니다.

# 예시

예를 들어, ANIMAL_INS 테이블과 ANIMAL_OUTS 테이블이 다음과 같다면

ANIMAL_INS

이미지

ANIMAL_OUTS

이미지

SQL문을 실행하면 다음과 같이 나와야 합니다.

이미지

# MySQL
```
SELECT i.ANIMAL_ID, i.NAME
FROM ANIMAL_INS AS i JOIN ANIMAL_OUTS AS o ON i.ANIMAL_ID = o.ANIMAL_ID
WHERE i.DATETIME > o.DATETIME
ORDER BY i.DATETIME;
```

# Oracle

AS를 사용해서 별칭을 정할 수 없다!! (오류뜸)
AS 제외하고 사용하니 됨!
INNER JOIN을 JOIN으로 적어도 됨
```
SELECT ins.ANIMAL_ID, ins.NAME
FROM ANIMAL_INS ins INNER JOIN ANIMAL_OUTS outs ON (ins.ANIMAL_ID = outs.ANIMAL_ID)
WHERE ins.DATETIME > outs.DATETIME
ORDER BY ins.DATETIME;
```

# 링크
https://programmers.co.kr/learn/courses/30/lessons/59043