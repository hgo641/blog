---
title: "SQL-DDL"
date: 2022-07-20
tags:
  - SQL
series: "SQL"
---

## DDL (Data Define Language)

> 데이터 정의어

DB를 구축하거나 수정할 목적으로 사용하는 언어이며, 총 세 가지 유형이 존재한다.

<br/>

* CREATE
* ALTER
* DROP



## CREATE

CREATE 명령어에 대해 자세히 알아보기전 알고있으면 좋을 것!<br/>

데이터베이스 서버 ⊃ 데이터베이스 스키마 ⊃ 데이터베이스 테이블<br/>

* 데이터베이스 서버는 데이터베이스들의 그룹화이고,
* 스키마는 테이블들의 그룹화이다.

<br/><br/>

* 표(= 릴레이션) : 행(=row=record=tuple) + 열(=column=attribute=field)

* degree : Attribute의 수

* cardinality : tuple의 수

* domain : 하나의 Attribute가 취할 수 있는 동일한 유형의 원자값들의 집합을 의미한다.(네?)

  * 쉽게 말하면 도메인은 특정 Attribute에서 사용할 데이터의 범위를 사용자가 정의하는 사용자 정의 데이터 타입이다.

  * ex) 만약 성별 속성의 값이 "남", "여" 라면 이 두 개의 값은 성별 속성의 도메인이다.

  * ex) 만약 학년 속성의 값이 1~4라면, 1~4가 학년 속성의 도메인이다.

    > 위 성별 속성에서는 타입이 문자열로, 원래라면 모든 문자열이 들어갈 수 있지만 범위를 지정했기에 "남"과 "여" 둘 중 하나만 허용된다.<br/>
    >
    > 학년 속성도 동일하다.





### CREATE SCHEMA

스키마는 MySQL을 보면 database 그 자체를 의미한다.<br/>

스키마를 생성하고 스키마 안에 db테이블을 생성한다.<br/><br/>

명령어 구조는 아래와 같다.

```sql
CREATE SCHEMA 스키마명 AUTHORIZATION 유저아이디;
```

예시 : CREATE SCHEMA `대학교` AUTHORIZATION `홍길동`

> 스키마 이름은 `대학교`이고, 소유권자의 유저아이디는 `홍길동`이다.



### CREATE DOMAIN



명령어 구조는 아래와 같다. []안의 내용은 생략가능하다.

```sql
CREATE DOMAIN 도메인명 [AS] 데이터타입
		[DEFAULT 디폴트값]
		[CONSTRAINT 제약조건명 CHECK(범위값)];
```

<br/><br/>

#### 📌 예시

```sql
CREATE DOMAIN SEX CHAR(1)
		DEFAULT '남'
		CONSTRAINT VALID-SEX CHECK(VALUE IN ('남', '여'));
```

> 도메인 이름은 `SEX`이고<br/>
>
> 디폴트값은 '남'이고<br/>
>
> 제약조건은 '남'과 '여' 둘 중 하나여야하며, 이 이름은 편의상 `VALID-SEX`라고 붙인다.



### CREATE TABLE

CREATE TABLE명령어의 옵션은 아래와 같다.

* PRIMARY KEY
* UNIQUE
* FOREIGN KEY ~ REFERENCE ~
  * ON DELETE
  * ON UPDATE
* CONSTRAINT [CHECK()]<br/><br/><br/>

형식보다 예시를 보는게 더 이해가 쉬워서 바로 예시로 넘아가겠다!

```sql
CREATE TABLE 학생
		(이름 VARCHAR(15) NOT NULL,
         학번 CHAR(8),
         전공 CHAR(5),
         성별 SEX, // SEX 도메인 사용
         PRIMARY KEY(학번),
         FOREIGN KEY(전공) REFERENCES 학과(학과코드)
         		ON DELETE SET NULL
         		ON UPDATE CASCADE,
         CONSTRAINT 생년월일제약
         	CHECK(생년월일 >= '1980-01-01'));
```



### CREATE VIEW

명령어 구조는 아래와 같다. []안의 내용은 생략가능하다.

```sql
CREATE VIEW 뷰명[(속성명, 속성명, ...)]
AS SELECT문
```

<br/><br/>

#### 📌 예시

```sql
CREATE VIEW 서울고객(성명)
AS SELECT 성명
FROM 고객
WHERE 주소 = "서울시";
```



### CREATE INDEX

인덱스는 검색 시간을 단축시키기 위해 만든 보조적인 데이터 구조이다.

명령어 구조는 아래와 같다. []안의 내용은 생략가능하다.

```sql
CREATE [UNIQUE] INDEX 인덱스명
ON 테이블명(속성명[ASC|DESC], 속성명, 속성명 ...)
[CLUSTER];
```

> 클러스터드 인덱스는 인덱스 키의 순서에 따라 데이터가 정렬되어 저장되는 방식이다.

<br/><br/>

#### 📌 예시

```sql
CREATE UNIQUE INDEX 고객번호_idx
ON 고객(고객번호 DESC);
```

