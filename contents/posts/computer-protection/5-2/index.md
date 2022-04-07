---
title: "보안 작성중"
date: 2022-04-03
tags:
  - computer protection
---

## AES

### AES structure

128bit input -> E -> 128 output

key값에 따라 output차이

Block Cipher : finite field 사용

why?

field : 사칙연산 덧셈 : randomize효과가 실질적으로 없음. 무작위로 바뀌지않고 계속 유지되는 성질 XOR 같은거 더하면 상쇄됨 연산을 반복한다고해서 복잡도가 올라가지않음

### Rationale for Binary Field

n비트의 int는 2^n 개 표현 가능

모듈러 2^n은 field가 될 수 없음 ex) 2^3 = 8

-> GF(2^n)사용 GF안의 모든 polynomial들이 n비트 넘버를 표현

GF기준으로 x2 + 1도 인수분해 가능.
irreducible polynomial :

DES 56bit크기의 key 키 크기가 작아서 취약

키의 크기도 늘리고 성능 유지한게 AES

AES에서 쓰는 기본 자료구조 : State

128bit byte를 16개(4x4) 128bit단위로 암호화됨 plain text, ct

key 비트수에 따라 반복되는 round수도 달라짐

round key == subkey

round key의 비트는 똑같지만 개수가 많아짐 : expanded key

첫라운드전 한 번 + 각 라운드별로 서브키가 하나씩 사용됨

라운드 개수+1만큼 필요

서브키가 16byte 라운드가 11일경우

16\*11 = 176

State (4x4)

: 평문을 열로 나열해서 자름

initial transformation

: 평문과 key를 xor

round

key expansion(key schedule) = 서브키를 만들어내는 과정

### Detailed Structure

---

결과적으로 AES에서 내부적으로 하는일은 전체적인 데이터의 블록(4x4)들을 substitution과 permutation을 이용해서 업데이트함
