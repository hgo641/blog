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

기존 (len(p))^3 => (len(p))^2 x len(q)



L : p

N : q



타원곡선은 GNFS가 적용이 안됨. 전체 가능한 지수의 개수가 몇 개냐만 공격과 관련이 있음

xG = Q일 때 x의 종류가 몇 개냐만 관련있음

q는 프라임이어야하고 폴링 헬만이라는 공격은 q의 인자들을 찾아서 각각에 대해서 따로 공격한다. 즉, q가 프라임이어야함



## Schnorr Identification

zero knowledge proof protocol (zkp)

일종의 챌린지 리스폰스 타입의 authentication (사용자인증)

챌린지 리스폰스? 

> verifier측에서 prover측에게 챌린지를 던짐
>
> ex) 비밀번호에 2를 더한 값을 보내라
>
> 같은 패스워드지만 챌린지에 따라 리스폰스 값이 달라짐

schonorr는 이 챌린지를 디스크리트 로그 문제를 기반으로 만듦

ex client = Alice, verifier = Bob

Bob은 Alice가 개인키를 알고있는 사람인지 확인

* alice가 랜덤한 넘버 k를 뽑음 (0부터 q-1사이의 수)

* k를 사용해 감마를 생성하고 Bob에게 전송

  * k는 일시적인 프라이빗키, 감마는 일시적인 퍼블릭키

* Bob이 Alice에게 랜덤한 챌린지 전송

* Alice는 받은 r을 가지고 y를 생성후 Bob에게 전송

* Bob은 y를 가지고 수식이 맞는지 확인

  > v^r은 a^-ar mod p이고
  >
  > a^y = a^k x a^ar mop이므로 둘을 곱하면 a^ar은 상쇄되고 a^k = 감마만 남는다
  >
  > 제너레이터 == 알파

<br/>

Alice는 Bob에게 자신의 프라이빗키를 알리고싶지않음

그러나 계속 인증정보를 주다보면 Bob이 Alice의 프라이빗키를 알 수 있지않을까?

No : zero-knowledge proof - 밥한테는 어떤 정보도 주지않음

(감마, r, y)

밥이 앨리스하고 얘기하지않고 혼자서 만들어낼 수 있는 순서쌍과 본질적으로 다르지않다. 앨리스로부터 받은 정보가 추가적인 정보라고 보기 힘들다. 제 3자가 보면 똑같은 정보.



## Schnorr Signature

* schnorr identification은 interactive proof protocol이다
  * 서로 왔다갔다 정보통신
* 인터렉티브 proof protocol을 전자서명으로 바꿔주는 방법 : Fiat-Shamir

밥이 어떻게 질문을 할 지 미리 앎 : r을 미리 앎

y도 미리 마음대로 정함

계산한 감마를 생성

=> 프라이빗키 a를 모르고도 통신 가능

=> 왜? 밥이 어떤 질문을 할 지 미리 알아서



r = H(M||감마)

r을 이렇게 만들면, 위에서 한 r을 먼저만들고 감마를 생성하는 법을 막을 수 있음...

H가 일종의 랜덤함수 역할

메시지에 기반해서 만든 r

Alice가 주는건(감마, y)

Bob이 r을 위의 식으로 만들고 확인



두 번째 서명 방법

감마가 아니라 (r, y)를 줌

밥은 r과 y를 사용해 감마'를 만듦

r = H(M||감마')



## NIST Digital Signature Algorithm

DSA=DSS

SHA가 포함됨



Schnorr처럼 p, q를 이용한 서브그룹 구조 사용

p-1의 약수가되는 또 다른 프라임 넘버 : q

q만큼의 원소를 가지고 있는 서브그룹을 정의...

지수를 작게 유지하면서 안전성은 해치지않는...

바깥의 모듈러에는 p를, 지수 모듈러에는 q를 사용



generator = g

x = 개인키

y = 공개키

* mod p 하고 또 mod q
  * q는 p보다 훨씬 작은 수
  * 서명의 크기를 줄여준다

(r, s)를 가지고 서명





서명확인?

w = 두 번째 서명값에 모듈러 역수

v와 r이 같은지 확인...



공격한다면?

공개키 y를 가지고 개인키x를 맞출 수 있다면 깨짐 (디스크리트 로그라서 깨기 힘듦)



DSA를 타원곡선버전으로...

디스크리트 로그는 y = g^x mod p일 때,

x를 맞추기 어렵다.

곱하기 연산을 하고 DSA의 경우 q를 사용해 q개 뽑아서 사용. 서브 그룹중에서 몇 번째냐. q번 반복하면 원래값으로 돌아옴

타원곡선은 dG = Q 일때 d를 맞추기 어렵다. 

지수

더하기 연산을 하고 1G 2G 3G... nG더하다 보면 원래 값으로 돌아오는 n이 존재.

상수



## RSA-PSS

> M^e, C^d이렇게 하면 choosen cipher text attack에 약하다 그러니 OAEP같은 패딩스킴을 쓰자

얘도 비슷

랜덤한걸로 패딩 스킴쓰는 그런거 PSS라고 부름

> 똑같은 메시지를 넣더라도 서명이 그때그때 바뀜
>
> salt가 랜덤화됨

FIPS 186표준에 포함



H'과 H가 같은지 확인 - 서명

