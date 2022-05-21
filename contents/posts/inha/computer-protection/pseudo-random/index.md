---
title: "7-2 난수 생성"
date: 2022-04-18
tags:
  - computer protection
series: "컴퓨터 보안"
---

## 난수 생성

> **난수는 왜 필요?**
>
> 그냥 대부분의 암호에 필요하다... 보통 key에 사용됨
>
> 암호의 안전성은 key에 기반하기때문에 key생성이 중요함

#### 난수의 조건

- Randomness

  `Uniform distribution` : 나올 수 있는 수들의 분포가 균일해야한다

  `Independence` : 서로 독립적. 긴 시퀀스로 봤을 때 부분 부분간의 연관관계가 없어야함. 반복되는 구간이 존재하지않는다.

- Unpredictability

  예측이 불가능해야한다

  어떤 구간을 보고 그 다음 구간을 예측할 수 없어야한다

### Pseudorandom Numbers

> 난수를 만드는 것은 자원이 많이 드는 일
>
> 진짜 랜덤은 아니지만 랜덤처럼 보이는 시퀀스를 만들어내는 알고리즘이 더 일반적
>
> Deterministic algorithm

- TRNG = 진짜 난수 생성기
- PRNG = 수도 난수 생성기 : 무한 stream 출력
- PRF = 수도 난수 함수 : 길이가 정해져있는 value 출력

### TRNG

- entropy를 만들어내는 함수로 entropy source라고도 불린다
- 물리적인 특성들을 가지고와서 사용하는 경우가 많다

 ex) keystroke timing, disk electrical activity, mouse movements

- 보통 TRNG로 seed를 만들고 PRNG를 돌려서 더 긴 시퀀스를 만들어낸다

- 아날로그 소스들을 받아서 바이너리 아웃풋으로 내보낸다

- 경우에 따라서 bias가 있기도 한다 : 이럴경우 bias를 처리해줘야함

### PRNG

- deterministic algorithm
- key stream을 만들어낸다
- input이 정해지면 output도 정해진다
- 그렇기에 seed값을 안전하게 잘 보호해야함

### PRF

- 길이가 고정된 아웃풋을 만들어낸다

## PRNG와 PRF의 조건

- Randomness
- Unpredictability
- Characteristics of the seed : seed를 안전하게 보관한다

진짜 난수는 아니지만 난수성을 만족

seed를 모르면 PRNG가 만들어낸 아웃풋이 랜덤처럼 보여야한다

## Randomness 정량적 평가 방법

- Frequency test: 0과 1이 몇 개씩 나오는지 확인

- Runs test : 같은 비트가 반복되면 안됨

- Maurer's universal statistical test : 시퀀스를 압축을 시켰을 때 잘 되는지

등등 15가지가 존재

## Seed Requirements

- 시드는 secure하고 unpredictable해야한다

- 수도랜덤넘버를 사용하기도하지만 시드는 보통 진짜 랜덤, TRNG로 만든다

## Algorithm Design

**PRNG 알고리즘 설계**

- 이미 존재하는 암호 알고리즘을 기반으로 만들거나
- 난수 생성 자체를 목적으로, 전용으로 만들어서 쓸 수도있음

## Linear Congruential Generator : 안전 x

- 많이 생성하는 난수 생성기이나 암호학적으로는 사용할 수 없음

- X0은 seed

- 1차식이기 때문에 위험

## PRNG Using Block Cipher Modes of Operation

- CTR mode
- OFB mode

### NIST CTR_DRBG

- Counter mode
- 굉장히 많이 쓰임. Intel cpu에서도 사용
- entropy source가 있어야함. entropy소스로 부터 seed를 받아서 카운터 기반의 PRNG를 돌려서 긴 수도 랜덤 시퀀스를 만들어낸다
- 3DES와 AES를 사용

## Stream Cipher Desing Considerations

수도 랜덤 생성 전용 스트림 함수

- period가 길어야 함. seed값이 같으면 계속 넘버를 생성할 경우 일정 싸이클이 반복되게된다
- true random number stream처럼 보여야한다. - 분포나 연관관계 없어보여야함
- key length가 최소한 128bit - 키가 짧으면 공격자가 추측해보기 쉬움
- 비슷한 key length를 가지는 블록싸이퍼와 비슷한 안전성을 갖길 기대

### RC4

키 사이즈 변경 가능

바이트 단위로 연산가능

SSL에 사용됐었음

후속버전임 TLS에서도 쓰였음

무선랜에서도 쓰였

**그러나 broken됐음...**

### ChaCha20

Salsa20암호를 살짝 바꾼...

## TRNG and Entropy Source

- nondeterministic
- Sound/Vide, Disk driver 등을 모아서 trun random number를 만들기위한entropy source로 사용한다

## Conditioning

entropy source가 있으면 다 해결?

bias가 있을수도

후처리필요

후처리를 해서 bias를 없앴을때 동전던지기를 몇번 한 효과가 있을것이냐를 디지털화된 노이즈 소스가 제공하는 난수성이 어느정도냐 entropy ratio

bias를 conditioning알고리즘으로 돌려서 랜덤 효과를 늘림

## Intel Digital Random Number Generator

bias가 있어도 AES를 돌리다보면 random한 것 처럼 변함

Ring Oscillator기반의 entropy source에다가 AES CBC MAC을 집어넣어서 컨디셔닝을 해서 TRNG를 만들고 AES couter mode를 돌려서 PRNG를 만든다

intel에서 이걸 써먹을 수 있는 라이브러리를 줌

