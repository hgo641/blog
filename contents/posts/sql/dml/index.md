---
title: "SQL-DML"
date: 2022-07-21
tags:
  - SQL
series: "SQL"
---

## DML (Data Manipulate Language)

> 데이터 조작어

데이터 베이스 사용자가 저장된 데이터를 실질적으로 관리하는데 사용되는 언어이다.<br/>

데이터베이스 사용자와 데이터 베이스 관리 시스템 간의 인터페이스를 제공한다.

<br/>

- SELECT
- INSERT
- DELETE
- UPDATE
- JOIN

<br/>

> SELECT와 JOIN은 분량이 길어 다른 포스트에서 설명하고 여기서는 INSERT, DELETE, UPDATE만 정리해보겠다!

## INSERT INTO ~

테이블에 새로운 튜플을 삽입할 때 사용한다.

```sql
INSERT INTO 테이블명[(속성, 속성, ...)]
VALUES (데이터, 데이터, ...)
```

<br/><br/>

#### 📌 예시

Attribute가 이름, 학번, 생일, 학과, 학년이 있는 학생테이블이 있다고 해보자.

```sql
INSERT INTO 학생 VALUES ('홍길동', '1111', '05/03/00', '컴퓨터공학과',1)
INSERT INTO 학생(이름, 학과, 학년) VALUES ('홍길동', '컴퓨터공학과',1)
```

<br/>

아래와 같이 SELECT문 사용도 가능하다.

```sql
INSERT INTO 1학년학생(이름, 학과)
SELECT 이름, 학과
FROM 학생
WHERE 학년 = 1
```

## DELETE FROM ~

```sql
DELETE
FROM 테이블명
[WHERE 조건] // 모든 레코드를 삭제할 때는 생략한다.
```

<br/><br/>

#### 📌 예시

```sql
DELETE
FROM 학생
WHERE 이름 = '홍길동'
```

## UPDATE~ SET~

> 특정 튜플의 내용을 변경한다.

```sql
UPDATE 테이블명
SET 속성명 = 데이터[, 속성명 = 데이터, ...]
[WHERE 조건]
```

<br/>

<br/>

#### 📌 예시

```sql
UPDATE 학생
SET 학과 = '의적학과', 학년 = 2
WHERE 이름 = '홍길동'
```
