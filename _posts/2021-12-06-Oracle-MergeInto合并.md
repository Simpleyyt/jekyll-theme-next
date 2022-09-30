---
title: Oracle-MergeInto合并
description: 使用Oracle时可以利用其语法特性MergeInfo实现数据的合并，该合并可以指定当数据匹配和不匹配情况下的具体操作，具有较高的自由度，同时减少了连续使用Select和Insert以及条件判断的概率，让编码更清晰
categories:
 - sql
 - oracle
tags:
 - oracle
 - sql
 - procedure
keywords:
  - oracle
  - sql
  - merge
  - mergeinto
  - plsql
  - procedure
  - select
  - insert
  - match
  - not matched
  - condition
  - 合并
  - 匹配
  - 不匹配
  - 条件判断
date: 2021-12-06 08:00:00
updated: 2022-09-30 00:00:00
---

## 前言

`merge into`语句的功能：我们操作数据库的时候，有时候会遇到**insert或者Update**这种需求。我们操纵代码时至少需要写一个插入语句和更新语句并且还得单独写方法效验数据是否存在，这种操作完全可以用`merge into`语句代替，不仅省时省力而且条理更清晰，一个SQL语句直接完成插入，如果有相同主键进行更新操作。

## 用法

`MERGE INTO`后是更新的表，`USING`是对接口表进行筛选，（如果有重复数据，仅选取一行插入，用`ORDER BY`控制）。`ON`中是具体的条件（表中标识字段，字段编码）满足执行`WHEN MATCHED THEN`下的语句，不满足则执行`WHEN NOT MATCHED THEN`后语句。

```plsql
MERGE INTO TableA A 
USING (
    (SELECT L.*, ROW_NUMBER() OVER(PARTITION BY T.FLEX_VALUE ORDER BY 1) AS RN
        FROM TABLEB L
    		WHERE T.BATCH_ID = #{batchId} ) L
    				AND L.RN = 1 )  B
ON ( A.FLEX_VALUE = B.FLEX_VALUE )
 		WHEN MATCHED THEN
  	UPDATE 
				A.FLEX_VALUE_SET_NAME = B.FLEX_VALUE_SET_NAME,
				A.VALIDATION_TYPE = B.VALIDATION_TYPE,
		WHEN NOT MATCHED THEN
  	INSERT (
    		A.FLEX_VALUE_SET_NAME = B.FLEX_VALUE_SET_NAME,
				A.VALIDATION_TYPE = B.VALIDATION_TYPE)
```
