---
title: "django ORM"
date: 2022-03-28
tags:
  - django
series: "askcompany"
---

## 장고 모델

<데이터베이스 테이블>과 <파이썬 클래스>를 1대1로 매핑

- **모델 클래스명은 단수형으로 지정**
  ex) Posts(x) Post(o)

- **첫 글자는 대문자** - PascalCase 네이밍

### 장고 모델을 통해 데이터베이스 형상을 관리할 경우

1. 모델 클래스 작성
2. 모델 클래스로부터 마이그레이션 파일 생성 : makemigrations
3. 마이그레이션 파일을 데이터베이스에 적용 : migrate
4. 모델 활용

### 장고 외부에서 데이터 베이스 형상을 관리할 경우

데이터베이스로부터 모델클래스 소스 생성 : inspectdb
모델 활용

## DB 테이블명

default : "앱이름\_모델명"  
ex) blog앱의 Post모델 -> "blog_post"
커스텀을 위해선 모델 Meta 클래스의 db_table 속성 변경

> migrate전에 변경해야함

## migrate 쿼리 보기

python manage.py sqlmigrate (앱이름) (마이그레이션파일이름)  
ex) python manage.py sqlmigrate blog 0001_initial

## 테이블 목록 보기

python manage.py dbshell (sqlite 설치 필요)  
.tables

## 스키마 보기

.schema (db 테이블명: 앱이름\_모델이름)  
ex) .schema blog_post
