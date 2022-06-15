---
title: "10-1"
date: 2022-05-02
tags:
  - computer protection
series: "컴퓨터 보안"
---



## Modular Exponentiation

* e는 고정된 작은 값을 쓸 수 있지만 d는 매우 큰 숫자를 사용하게 된다
* n의 bit수와 d의 bit수는 거의 같다
  * n은 3072 or 4096 bit 자리 이진수

* n = p x q (p, q는 각각 2048 bit) 곱하면 4096bit
* 파이n은 (p-1)(q-1)이므로 역시 4096bit
* d는 e의 곱셈의 역원을 모듈러 파이n한 것이므로 : 0 < d < 파이(n)
* d의 자릿수 숫자는 불규칙?
  * d의 앞 bit가 1이면 4096bit (확률 1/2)
  * d의 앞 bit가 0, 두 번째 bit가 1이면 4065bit (확률 1/4)
  * 이런식을 확률이 점점 줄어듦. 즉 bit수가 4096과 가까울 확률이 높음


* Two key techniques for fast exponentiation
  * 거듭해서 곱하지말고 제곱과 곱하기를 해결하자~
  * a^b는 숫자가 너무 크니, 각 a에 mod n연산을 해줘서 크기를 줄이자!
    * 4000 -> 8000 -> 16000 -> 24000 ...
    * 4000 -> 8000 -> 4000(mod n) -> 8000 ...
  * 어차피 (x x y) mod n = [(x mod n) x (y mod n)] mod n
  * 이게 적용된게 `Fast Modular Exponentiation` 알고리즘



## Binary L to R

=(Square and multiply)

* 이진수로 분해
* 제곱 (f x f)
* 비트 곱하기? (f x a)

제곱과 선택적인 비트 곱하기



## Application of RSA

* 기밀성(Confidentiality)
  * src는 dest의 pu로 암호화
  *  dest는 dest의 pr로 복호화

* 인증(Authentication) - 전자서명(nonrepudation) 제공 가능
  * src는 src의 pr로 암호화
  * dest는 src의 pb로 복호화

* 기밀성 & 인증 둘 다
  * src가 src의 pr로 암호화 (서명)
  * src가 dest의 pu로 암호화(메시지 암호화)
  * dest가 dest의 pr로 복호화(메시지 복호화)
  * dest가 src의 pb로 복호화 (서명확인)
