---
title: "12-2"
date: 2022-06-10
tags:
  - computer protection
series: "컴퓨터 보안"
---

## Message Authentication Code

맥의 요구조건, 기능

Masquerade - 상대인척 속임

메시지 변조, 없는 메시지 생성, 메시지 순서 변경, 시간값 변조등을 막기위해 맥을 사용

인증가 무결성을 동시에 제공하는 것이 MAC!

> 크게 보면 입력은 두 가지 존재
>
> message와 secret key
>
> 보통의 암호화L와 비슷
>
> 아웃풋이 암호문이면 암호화, 무결성을 체크하기위한 코드값이 나오면 맥
>
> 인증을 위해 뒤에 덧붙이는 값을 authenticator라고함

정해진 길이의 인증자(인증 태그) 생성

Mac = keyed hash

![](./three-example.png)

C는 블록 싸이퍼의 CBC모드일수도 해시함수일수도 있음

## HMAC

해시기반으로 만든 맥

안전성에서 큰 차이가 없다면 DES말고 더 빠른 해시함수쓰자~

IP security - transport층에서 사용자 보호(TES) 그 아래의 네트워크 계층에서 가장 많이쓰이는 IP 프로토콜을 보호하는 표준 프로토콜

니스트 표준

### 절차

- K를 블록으로 나눔
  - 만약 K와 블록의 사이즈가 같다면 그냥 바로 함
  - K가 더 크면 L byte string이 되도록 해시를 해줌. 나눴는데 L byte가 B에 비해 모자라면 0으로 채워줌 (B-byte가 되게)
  - 더 작으면 0을 부여서 B-byte가 되게 함
- K0은 B와 크기가 같음. ipad와 xor

  - ipad : 상수, 16진수로 36, 1바이트

- text를 붙임
  - text : MAC을 생성할 대상
- 해시 돌림
- K0과 opad xor
  - opad : 5c라는 바이트 값을 쭉 반복한 값
- 두 개를 붙이고
- 붙인걸 또 해시

## CMAC

cbc 모드를 가지고 맥을 만들 수 있다

- 그걸로 나온게 DAA

cbc의 initial vector빠짐

문제

만약 K와 플레인텍스트 X를 넣는데 X가 한 블럭짜리라면

- 공격자가 임의로 MAC을 만들어낼 수 있음
- 두 블럭짜리 플레인텍스트를 만든다
  - 첫번째 플레인텍스트 블록 : X
  - 두 번째 : X xor T
- cbc-mac 연산 돌리면 똑같이 T나옴

이걸 해결한게 CMAC

마지막(두 번째) 부분에 대한 CBC를 돌릴 때는 다른 키를 더 써서 xor을 한 번 더 시킴

> M의 길이가 작아 패딩이 있는 블록이라면, (패딩 : 1하나빼고 다 0)
>
> K1이 아니라 K2라는 다른 키를 적용
>
> K1과 K2는 처음 입력받은 K를 가지고 유도

K를 0만 있는 것과 암호화 (B byte string)

L x X = L의 비트를 왼쪽으로 옮김

## Authenticated Encryption

confidentiality와 authentication을 둘 다 제공

하는 방법

- Encryption 한 다음에 해시 : 안전성 보장 안됨
- 인증(맥)후 암호화 : 앞에서 나옴
- 암호 따로 인증 따로 등등...

### CCM (Counter with CBC-MAC)

카운터 모드 Encryption

사실 CBC맥 안쓰고 CMAC사용

인크립션과 맥을 따로따로 하고 붙임

AES 암호 알고리즘 사용

암호화와 맥에 같은 key를 쓰도록 되어있음

세 가지 데이터 종류

- 인증도 하면서 암호화도 해야하는 데이터 : 페이로드가 되는 데이터 블록
  - ex) IP패킷. 페이로드 내용은 중요해서 암호화되어야하지만 IP 주소 정보같은건 암호화 되면 주소 못찾음;; 페이로드만 암호화 필요
- 암호화할 필요는 없지만 인증이 필요 : associated data
- replay 어택을 막기위해 매번 보낼때마다 바뀌는 값을 만들어줌(nonce)
  - reply 어택 : 실제 사용자가 여러번보낸건지... 공격자가 복제해서 여러번 보낸건지..
  - data가 같은데 nonce까지 같으면 비정상적인 메시지

![](./ccm.png)

인증

세 가지 데이터를(nonce, ass.data, plaintext) CMAC을 돌려서 Tag를 얻음

암호화

plaintext에만 암호화 필요

카운터사용

막판에 인증과 암호화 아웃풋 합침(Tag xor MSB(Tlen))

인증과 암호화를 독립적으로 적용, 완료후 합침

- MSB 만약 CMAC에서 특정 길이 아웃풋을 준다면 MSB에서 most, 최상위비트부터 잘라서 전달

### GCM (Galois Counter Mode)

CCM은 병렬화가 안된다는 성능상의 문제가 존재(카운터모드는 병렬화 가능, CMAC은, CBC모드는 안됨. 앞에 값을 뒤에 전달.)

GCM은 앞과 뒤의 연관 관계를 만들어주면서도 병렬화가 가능

- CTR모드로 암호화

- 암호문을 key material로 authenticator tag생성

  - 암호화 이후 태그를 붙여주는 방식(갈루아필드사용)

  > 근데 사실 맥만 만들수도있음. 카운터 모드 빼고 맥으로만 (GMAC)

두 가지 모드 사용

- GHASH - MAC용으로 사용, keyed hash function

- 카운트 모드를 살짝 바꾼 GCTR - 연산 하나 증가할 때마다 하나씩 증가

![](gcm.png)

> H는 128bit의 암호문
>
> H를 GF(2^128)의 원소로 본다
>
> 플레인 텍스트인 X역시 원소로 봄
>
> (127차까지 가능한 다항식이 됨)
>
> CBC모드와의 차이 : 키를 사용하는 Block Cipher Encryption 대신에 k를 사용해 생성한 H(keyed hash)를 곱하는걸로 바뀜

병렬이 안되는 것 처럼 보이지만 병렬이 가능(H를 곱하므로 인수분해처럼 빼서 병렬 가능)

GCTR

마지막 블록은 정해진 블록 사이즈 크기보다 작을 수 있기에 MSB 적용

전체적으로 암호화 -> 인증 구조

## GCM and intel CPU

빨리 돌리기위해 연산을 하드웨어로 구현해서 제공하고 있음

최근에 나오는 cpu는 PCLMULQDQ라는 instruction을 제공한다 - 캐리를 위로 올리지않는 곱셈- 두개의 64 오퍼랜드를 받아서 캐리없이 연산해줌 - binary field 연산

1. 타원곡산연산 점의 계산에서 바이너리 필드를 사용할 때 사용 - 그러나 최근 바이너리필드를 사용하는 타원곡선의 약점 발견..., 프라임필드를 쓰는 추세
2. GCM Mode : GCM 빨리 돌리기 가능

## 암호 모듈 검증 제도

KCMVP라고 부름

> 공공기관에 납품되는 암호품은 여기서 검사받고 검증되어 들여져야함

nis.go.kr에서 자세히 확용 가능

블록암호 : AES없음(우리나라)

LSH - 국산 해시 함수
