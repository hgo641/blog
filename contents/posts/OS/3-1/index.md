---
title: "3-1"
date: 2022-04-20
tags:
  - OS
series: "OS"
---

## Compilation System

- Pre-processing : c코드 형태, #include, define등 전처리
- 컴파일러 : 어셈블리어 형태로
- 어셈블러 : 기계어 형태로
- 링커 : 여러 코드파일들을 하나의 실행파일로 합쳐줌 ex. printf.o + hello.o

### excutable 파일

text segement: 기계어

data segment : global 변수들, read only data(string), system data

%rbx : 레지스터

(%rbx) : 메모리

## Stack

- 지역변수나 함수간에 주고받는 인자등을 저장해둠
- 레지스터에 모든 데이터를 다 담아둘 순 없으니까
- top 방향으로 쌓이고 없어질때도 top부터 없어짐
- bottom이 address high임 띠용
- top의 주소만 알면 push, pop가능

- **%rsp가 stack의 top을 가리키고 있다**

- push src

  %rsp 8 감소

- pop dst

  %rsp 8증가
