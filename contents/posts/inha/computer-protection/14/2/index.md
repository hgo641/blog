---
title: "14-2"
date: 2022-06-15
tags:
  - computer protection
series: "컴퓨터 보안"
---

## Certivicates

앞의 퍼블릭키 분배 방법은 다 중간에서 누군가 속일 수가 있음

믿을 수 있는 기관을 이용하자!

Public key authority

* E(PRauth, PUb Reauest||T1)
  * PUb가 밥의 퍼블릭키가 맞다는걸 authority가 자신의 프라이빗키로 인증



근데 꼭 authority가 중간에서 매번 전달해줄필요없음.

인증서를 미리미리 받아놓자?

어차피 authority 프라이빗키라 보내는 사람이 조작할 수 없음

> Issuer name 은 CA의 name
>
> Subject name은 Alice냐 Bob이냐. 누구의 퍼블릭키인지



## X.509 Certificates

저 인증을... 표준화해놓은거

X.500이 directory service 표준

인증서에 관한 디렉토리가 x.509

각 certificate에는 특정 사용자의 public key가 들어있음. 아이디(user)와 퍼블릭키 쌍에다가 믿을 수 있는 certification authority의 프라이빗키로 위의 쌍이 서명된 형태



## 인증서로 MIM어택 막기

Z : 지금 보내는 임시공개키 YA가 A가 만든게 맞다는 서명

C : 퍼블릭키 인증서

다스가 공격을 성공하려면,

c와 z중 하나라도 위조할 수 있으면 된다.



* z를 위조하려면 YD1을 넣고 앨리스의 프라이빗키로 서명할 수 있으면 된다.
* 물론 프라이빗키 모름
* C 위조하려면 CA의 프라이빗키 알아야함



## 인증서가 가지는 성질

CA의 퍼블릭키를 알고있는 누구나 상대방의 퍼블릭키를 확인할 수 있다

CA외에는 certificate를 조작할 수 없다

즉, C는 아무나 볼 수 있는 디렉토리에 올려놔도됨



반대로 CA의 공개키가 없으면 인증불가능

> X.509의 PKI (Public Key Instructure)로 해결 (X.509 CA Hierarchy)

상위 인증 기관





