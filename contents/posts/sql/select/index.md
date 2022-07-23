---
title: "SQL-SELECT"
date: 2022-07-23
tags:
  - SQL
series: "SQL"
---

## SELECT (1)

```sql
SELECT [DISTINCT]
FROM
[WHERE]
	[GROUP BY]
	[HAVING]
[ORDER BY]
```

#### 📌 예제

```sql
SELECT 이름 + '의 월급' AS 이름2, 기본급 + 10 AS 기본급2
FROM 사원
```

```sql
SELECT *
FROM 사원
WHERE 부서 IN ('기획', '마케팅');
```

```sql
SELECT *
FROM 사원
WHERE 이름 LIKE '김_';

SELECT *
FROM 사원
WHERE 이름 LIKE '김%';

SELECT *
FROM 사원
WHERE 경력 LIKE '1#';
```

```SQL
SELECT *
FROM 사원
WHERE 생일 BETWEEN #010169# AND #12/31/73#
```

```SQL
SELECT *
FROM 사원
WHERE 주소 IS NULL

SELECT *
FROM 사원
WHERE 주소 IS NOT NULL
```

```sql
SELECT TOP 2 *
FROM 사원
ORDER BY 경력 DESC, 생일 ASC;
```

하위질의 : 조건절에 주어진 질의를 먼저 수행하여 그 검색 결고를 조건절의 피연산자로 활용한다.

```sql
SELECT 이름, 주소
FROM 사원
WHERE 이름 = (SELECT 이름 FROM 여가활동 WHERE 취미 = '코딩')

SELECT 이름, 주소
FROM 사원
WHERE 이름 NOT IN (SELECT 이름 FROM 여가활동)

SELECT 이름, 주소
FROM 사원
WHERE EXISTS (SELECT 이름 FROM 여가활동 WHERE 여가활동.이름 = 사원.이름)
```

> EXIST : 하위질의로 검색된 결과가 존재하는지 확인할 때 쓴다...
>
> 위 예시의 경우 사원테이블의 이름이 여가활동의 이름에도 있는지 확인하고, 두 테이블에 이름이 존재하는 사원의 이름과 주소를 출력한다.

## SELECT (2)

## 그룹함수

> GROUP BY 절에 지정된 그룹별로 속성의 값을 집계할 함수를 기술함

- COUNT
- SUM
- AVG
- MAX
- MIN
- STDDEV : 표준편차
- VARIANCE : 분산
- ROLLUP
- CUBE

#### 📌 예시

```sql
SELECT 부서, AVG(상여금) AS 상여금평균
FROM 상여금
GROUP BY 부서
```

> `상여금 `테이블에서 `부서`별 `상여금`평균을 구한다.

```sql
SELECT 부서, COUNT(*) AS 사원수
FROM 상여금
WHERE 상여금 >= 100
GROUP BY 부서
HAVING COUNT(*) >= 2
```

> `상여금 `테이블에서 `상여금`이 `100`이상인 사원이 `2명`이상인 `부서`의 튜플 개수를 구한다.

## WINDOW 함수

> GROUP BY절을 이용하지 않고 함수의 인수로 지정한 속성의 값을 집계한다
>
> WINDOW함수의 범위는 PARTITION BY절에 지정한 속성이 된다.

```sql
SELECT
[WINDOW함수 OVER (PARTITION BY 속성 ORDER BY 속성)]
```

<br/>

- ROW_NUMBER() : 각 레코드에 대한 일련번호 반환
- RANK() : 공동 순위 반영
- DENSE_RANK() : 공동 순위 무시

#### 📌 예시

```sql
SELECT 상여내역, 상여금, ROW_NUMBER() OVER (PARTITION BY 상여내역 ORDER BY 상여금 DESC) AS NO
FROM 상여금
```

> `상여금` 테이블에서 `상여내역`별로 `상여금`에 대한 일련번호를 구한다.
>
> 순서는 내림차순이며, 속성명은 NO

```sql
SELECT 상여내역, 상여금, RANK() OVER (PARTITION BY 상여내역 ORDER BY 상여금 DESC) AS 상여금순위
FROM 상여금
```

### 집합연산자

- UNION : 합집합
- UNION ALL : 중복허용 합집합
- INTERSECT : 교집합
- EXCEPT : 차집합

<br/>

#### 📌 예시

```sql
SELECT *
FROM 사원
INTERSECT
SELECT *
FROM 직원
```

## JOIN

> JOIN은 두 개의 릴레이션에서 연관된 튜플들을 결합하여, 하나의 새로운 릴레이션을 반환한다.
>
> - 일반적으로 FROM절에 기술한다.
> - 크게 INNER JOIN과 OUTER JOIN으로 구분된다.

### INNER JOIN
