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

#### AES에서 사용되는 연산

- Substitution : 3가지
- Transposition(permutation) : 1가지
- Multiple rounds : 여러 라운드로 진행됨
- Simple bit operations (such as XOR)

#### 궁극 목적

**Bit randomizion effect**  
Block Cipher : finite field 사용
비트를 랜덤해보이도록 만들어서 최대한 연관관계가 없어보이게

- Field multiplication과 Field inversion을 통해 랜덤하게 변형
  -> 그래서 finite field를 사용한다!
  _덧셈(뺄셈)은 반복하면 같아지는 효과가 있어서 제외_

why?

field : 사칙연산 덧셈 : randomize효과가 실질적으로 없음. 무작위로 바뀌지않고 계속 유지되는 성질 XOR 같은거 더하면 상쇄됨 연산을 반복한다고해서 복잡도가 올라가지않음

### Rationale for Binary Field

n비트의 int는 2^n 개 표현 가능
그러나 모듈러 2^n은 field가 될 수 없음 ex) 2^3 = 8
-> GF(2^n)사용 GF안의 모든 polynomial들이 n비트 넘버를 표현

irreducible polynomial :
(GF기준으로 x2 + 1도 인수분해 가능.)
![](./ex.png)

1. 모드2 이므로 2x 추가
2. `+`와 `-`가 같으므로 `x^2 - 1`로 변환

### AES?

Advanced Encryption의 표준
DES : 56bit크기의 key 키 크기가 작아서 취약
키의 크기도 늘리고 성능 유지한게 AES

#### State

AES에서 쓰는 기본 자료구조  
128bit byte를 16개(4x4) 128bit단위로 암호화됨 plain text, ct

round key == subkey  
round key의 비트는 똑같지만 개수가 많아짐 : expanded key

![](./key.png)  
key는 bit 수가 다른 여러가지 key가 있지만 plaintext는 항상 128bit로 고정이다  
key 비트수에 따라 반복되는 round수도 커진다  
round수가 늘어나면 라운드마다 쓰일 서브키(라운드키) 역시 증가한다

첫라운드전 한 번 + 각 라운드별로 서브키가 하나씩 사용됨  
라운드 개수+1만큼 필요  
서브키가 16byte 라운드가 11일경우  
16\*11 = 176

### AES Encryption Process

---

![](aes-process.png)
State (4x4)  
: 평문을 열로 나열해서 자름

**initial transformation**  
: 평문state와 key state를 xor

**round**

**key expansion(key schedule)** = 서브키를 만들어내는 과정

### Detailed Structure

---

결과적으로 AES에서 내부적으로 하는일은 전체적인 데이터의 블록(4x4)들을 substitution과 permutation을 이용해서 업데이트함

key가 128bit = 4 word일 때,  
필요한 라운드키는 4\*11 word 총 44개가 필요  
이를 w[0] ~ w[43]으로 표현할 것임

![](./aes-ed.png)
구현상의 편의성때문에 마지막 프로세스에선 Mix columns없음

### 4개의 프로세스

---

#### 1. Sub Bytes

---

![](./subbyte1.png)
byte단위로 업데이트. 치환을 해준다.
![](./s-box.png)  
S-box각 바이트들을 다른 바이트로 대체해주는 규칙  
모든 S-box사용자들이 같은 규칙을 씀(key의 일부가 아니라 정해진 방법) : 서로 다른 8bit가 다른 8bit와 1대1연결되어있다
