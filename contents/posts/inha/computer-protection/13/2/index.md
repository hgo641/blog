---
title: "13-2"
date: 2022-06-14
tags:
  - computer protection
series: "컴퓨터 보안"
---

## 전자서명 특성&요구조건

* 특성
  * 누가 언제 보냈는지 확인가능
  * 서명이 일어날 때 그 당시 메세지 콘텐트에 대한 확인
  * 제 삼자에게 증명이 가능함
* 요구조건
  * 보낸 사람이 부인할 수 없어야함
  * 개인키가 없으면 forgery할 수 없음
  * 서명 생성이 쉬워야함
  * 서명이 있을 때 확인하는 것도 쉬워야함
  * 프라이빗키가 없으면 서명이 생성하는게 어려워야함
  * 이미 있는 서명을 가지고 새로운 서명을 만들어내는게 어려워야함
  * 메시지가 있을 때 거짓된 서명을 만들어내는 것도 어려워야함



> Digital signature generation algorithm : 
>
> S = H(M)^d mod n
>
>  Digital signature verification algorithm : H(M) = S^e mod n





## DLP and Cyclic Group

ex)

8^8 mod 19 = 8^2 mod 19

> 8의 디스크리트 로그는 6제곱하면 다시 1로 돌아옴
>
> 8 mod 6 = 2



## Why subgroups are used?

DLP based들은 modular exponentiation 사용

>  x를 주고 값을 구하는게 modular exponentiaion
>
> 나머지를 보고 x를 구하는게 DLP

지수연산의 복잡도

* 반복되는 곱셈으로 구성됨. 곱셈의 개수가 지수 x 길이에 대체로 비례한다
* p의 비트길이의 제곱정도에 비례

> 효율성 측면에선 작은 p를 쓰는게 유리
>
> p가 커지면 퍼포먼스는 떨어지고 안전성은 올라감

<br/>

디스크리트를 푸는 대표적 알고리즘 두 가지

* General Number Field Sieve (GNFS)

  * mod p로 구성되는 prime field의 대수적인 성질을 이용?

  * 이 알고리즘의 복잡도는 p의 비트길이가 뭐냐에 따라서 결정된다

  * ex) 3072bit의 p를 사용할 때 2^128정도의 operations

  * > 원래 2^2072 정도인데 mod p의 prime field 성질을 이요해서 저 정도로 줄일 수 있다는 뜻인듯

* Pollard rho

  * 해시의 충돌을 찾는것처럼 birthday paradox를 사용
  * 디스크리트로그문제뿐만아니라 타원곡선에도 사용됨
  * 이 알고리즘의 복잡도는 x가 몇 가지 정도가 가능하냐에 달려있다
  * ex) 만약 2^256개 정도의 x후보가 있다면 그 것의 루트값인 2^128정도의 operations
  * 굳이 큰 모듈러를 쓸 필요가 없다.
  * q가 x 후보들의 개수가 됨??? (x mod q해서 0~q-1만큼의 후보가 있음)

두 가지 공격에 대한 대응책

p를 크게하게 q는 적당한 크기로 

* p는 여전히 크니 GNFS대응
* 2^256개 정도 나오면 괜찮음. 대충 2^256개정도 만들 수 있는 q를 골라서 Pollard rho 대응

