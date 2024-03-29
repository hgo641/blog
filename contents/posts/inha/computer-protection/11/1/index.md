---
title: "11-1"
date: 2022-06-06
tags:
  - computer protection
series: "컴퓨터 보안"
---









## 상호 암호제품의 국방 활용

암호 제품이란?

* 좁은 의미
  * 정보의 기밀성, L무결성, 인증 및 부인 방지를 제공하기 위해 암호기술을 적용하여 개발, 생상, 유통되는 제품

* 넓은 의미
  * 최근 ICT 융합 서비스가 확대되고 IoT 기기, GW등에 암호기술이 적용. 정보, 물리보안제품 및 암호, 인증 기술을 적용한 ICT제품까지 포함



### 발생 가능 문제점

작전용 활용시 발생가능 문제점

* 드론 탐지 시스템

> ex
>
> DJI 드론 탐색 가능
>
> 기체의 비행경로 및 조종자(부대) 위치를 알아낼 수 있음
>
> 통신 링크 가로채서 작동

* 통신상 보안문제
  * 상대의 휴대전화로 위치 파악

암호 제품을 사용할 때 검증이 중요

* 검증 체계 : KCMVP
* 국내용, 국제용 적합성 평가 존재



ex) 군 도입 상용 드론

드론을 구입해올때 KCMVP 암호 인증을 거친걸 가져옴

* 그 외, IC칩 정보 노출
  * IC칩 정보를 육안으로 확인할 수 있으면 해당 제품사의 제품 특징으로 드론의 정보를 얻을 수 있기에 위험
* 펌웨어 덤프
  * 디버깅 포트 연결하면 펌웨어 덤프도 가능
  * 내부 시스템 상태 노출 위험ㅌ
* 디버그 포트 접속
  * 디버깅 포트를 이용해 내부 파일 시스템에 접근 가능
* 부채널 대응 기술 부재

등의 문제가 있는지 또 검사



<br/>

이외에 암호적으로도 검사

* 암호 구현 오류
  * 동일 평문에 대해 보안에 취약한 블록암호 운영모드 사용(ECB같은거 하면 안됨)
* 암호키 평문 저장
* 무결성 검증키 노출
* 기기인증 기능 x
* 환경설정 파일 임의 수정



보안을 위해 국방 드론 보안 가이드라인을 만들어서 군 내에서 배포



진짜 설마 군무원, 경채에서 문제 내겠냐? ...

코드에서 문제가... 나올까?





## Buffer Overflow

> 메모리를 직접 만지는 함수때문에 생기는 문제
>
> ex) strcpy

* 잘못 수행하면 메모리의 데이터가 차지하는 영역이 망가짐.
* 바운더리를 체크하지않았기때문
* 오버플로우 되어서 원하지않는곳에 데이터가 쓰여짐
* 버퍼 overrun이라고 부르기도함
* 코드 자체를 공격하는 공격법중에 가장 많이 사용되는 방법
* c 나 c++처럼 메모리세이프하지않은 언어들에게서 생기는 문제. python같이 메모리를 다루지않는 언어에서는 문제가 발생하지않음
* 스택오버플로우와 힙오버플로우 존재



### Stack Buffer Overflow

> 공격자가 데이터를 제어하면서 데이터가 쓰여져야하는 영역 너머로 벗어나면서 어떤 함수의 return address를 덮어씌우면서 발생하는 문제

예시코드를 보면 

`strcpy(c, bar)`에서 strcpy가 몇 글자를 붙여넣을건지 정의가 안되어있음(오버플로우 가능 - 12글자가 넘어도 카피)

오버플로우가 된 세 번째 사진을 보면 return address가 저장되는곳까지 덮어씌워짐

잘못된 return address로 가게됨 (잘못된 것을 실행 - x-08~가 실행됨)

* 만약 리턴 어드레스가 유효하지않는 주소라면 윈도우라면 프로세스 죽음, 맥,리눅스라면 segmentaiton fault
* 실제 공격에선 프로그램이 그 영역으로 가도록(return address 수정), Shellcode가 실행되게 됨

* Shellcode
  * 길면안됨 - 짧은 코드
  * 명령어 인터페이스를 띄워줌 (shell을 만듦)
  * 만들어진 shell에 공격자가 또 다른 공격을 가할 수 있음
* 그럼 공격자가 어느 부분의 코드가 return address에  쓰여지는지 어떻게 추측할 수 있을까?

#### NOP sled

nop : 아무것도 안하는 명령

sled : 썰매

nop막 해두고 점프하게 해둠. 점프점프 하다가 shell code에 갈 수 있게 ^^



### 막는 방법

#### safe coding

코딩 단계에서 막는 방법

메모리 직접 안만지는 언어 쓰자! ㅎㅎ

성능때문에 메모리 만지는 언어 써야할 때

* safe libraries를 쓰자
* strcpy, gets ... 쓰면 안됨
* scanf_s 써라!
* 예전에 모리스웜이 finger protocol사용해서 악용



#### OS, compiler단계에서 막기

* IDS, IPS에서 들어오는 패킷 분석 - 특정 코드 패턴이 있는지 확인 (nop많은가?)
* 그걸 또 공격자가 알아채면 자연스러운nop로 공격 더하기연산 하고 답은 안씀
* 그걸 또 방어자가 알면 규칙을 업데이트하는...
*  스택 사용 불가능
* 스택 이상하게 되면 체크



###  취약점 찾는 방법

정적 분석 : 실행안하고 확인

* strcpy썻네 안돼!
* 데이터 플로우 분석
  * cotrol flow graph : 4개의 코드블록 + 분기  - 원래 가야하는 분기가 아닌 엉뚱한 분기로 갔는지 체크
  * Taint analysis : 얼룩, 뭔가가 묻었는지 확인, 오염 전파된 변수 찾음

, 동적 분석 - 실행을 시키면서

* Instrumentation
  * 원래 바이너리 코드 사이사이에 임의로 체크하는 코드를 집어 넣음

