---
title: "SQL-DCL"
date: 2022-07-21
tags:
  - SQL
series: "SQL"
---

## DDL (Data Control Language)

> 데이터 제어어

데이터의 보안, 무결성, 회복, 병행 제어 등을 정의하는데 사용하는 언어이다.<br/>

데이터베이스 관리자가 데이터 관리를 목적으로 사용한다.

<br/>

- COMMIT : 트랙잭션 수행 결과를 실제 물리적 디스크에 저장하고, 관리자에게 데이터 베이스 조작 작업이 정상적으로 완료되었음을 알림.
- ROLLBACK : 데이터베이스 조작 작업이 비정상적으로 종료되었을 때 원래의 상태로 복구함
- GRANT : 데이터베이스 사용자에게 사용 권한 부여
- REVOKE : 데이터 베이스 사용자의 사용 권한 취소



## GRANT, REVOKE



### 사용자 등급 지정 및 해제

```sql
GRANT 사용자등급 TO 사용자아이디리스트
REVOKE 사용자등급 FROM 사용자아이디리스트
```

<br/><br/>

#### 📌 사용자 등급

* DBA : 데이터베이스 관리자
* RESOURCE : 데이터베이스 및 테이블 생성 가능자
* CONNECT : 단순 사용자

<br/><br/>

#### 📌 예시

```sql
GRANT CONNECT TO HONG
```

> 사용자 아이디가 `HONG` 인 사람에게 단순히 데이터베이스에 있는 정보를 검색할 수 있는 권한을 부여



### 테이블 및 Attribute에 대한 권한 부여 및 취소

```sql
GRANT 권한리스트 ON 개체 TO 사용자 [WITH GRANT OPTION]
REVOKE [GRANT OPTION FOR] 권한리스트 ON 개체 FROM 사용자 [CASCADE]
```

* 권한 종류 : ALL, SELECT, INSERT, DELETE, UPDATE
* WITH GRANT OPTION : 부여받은 권한을 다른 사용자에게 다시 부여할 수 있는 권한
* GRANT OPTION FOR : 다른 사용자에게 권한을 부여할 수 있는 권한을 취소함
* CASCADE : 권한 취소 시 권한을 부여받았던 사용자가 다른 사용자에게 부여한 권한도 연쇄적으로 취소함

<br/><br/>

#### 📌 예시

```sql
GRANT ALL ON 고객 TO HONG WITH GRANT OPTION
```

> 사용자 아이디 `HONG`에게 `고객`테이블에 대한 모든 권한과 다른 사람에게 권한을 부여할 수 있도록 허용

<br/><br/>

```sql
REVOKE GRANT OPTION FOR UPDATE ON 고객 FROM HONG
```

> 사용자 아이디 `HONG`에게 `고객`테이블에 대한 권한 중 다른 사용자에게 UPDATE권한을 부옇라 수 있는 권한만 취소한다.



## COMMIT, ROLLBACK, SAVEPOINT

> 커밋 명령을 수행한 시점 전으로는 롤백 명령으로도 되돌릴 수 없다.

<br/>

#### 📌 예시

```sql
DELETE FROM 사원 WHERE 사원번호 = 1;
COMMIT;

// << ROLLBACK

DELETE FROM 사원 WHERE 사원번호 = 2;
SAVEPOINT S1;

// << ROLLBACK TO S1;

DELETE FROM 사원 WHERE 사원번호 = 3;

ROLLBACK TO S1; // SAVEPOINT S1을 설정했던 시점으로 돌아간다
ROLLBACK; // 최근 커밋 이후로 돌아간다.
```

