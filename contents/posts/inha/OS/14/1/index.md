---
title: "os 14-1"
date: 2022-06-12
tags:
  - OS
---

## Demand Paging

> 프로세스가 필요할 때마다 그 때 그 때 올려놓는다

### Valid-Invalid Bit

v : 메모리에 있는 페이지다~ vs i : 없어서 HDD가야한다~

* i인 경우 두 가지

  * 유효하긴 한데 메모리에 지금 없음

  * 진짜로 무효; (stack과 힙사이 비어있는 공간)
  * i인것만보고 두 가지 중 무엇인지 알 순 없음

하드웨어 MMU는 i인걸보면 그냥 page fault라는 정보만 전함

OS는 page fault에 대한 동작(대응)을 함



### Page Table Example

swap space(HDD, backing store)

OS는 page fault를 받으면 HDD에서 해당 정보를 가져와 physical memory의 빈 공간에 위치시킨다





### Page Fault

i이면, OS에게 Page Fault라는 트랩을 발생시킴

1. OS는 i가 실제 Invalid라서 난건지 그냥 메모리에 없는건지 판단함 (Invalid라면 abort)
   * 어떻게 앎?
   * PCB와 함께 table을 만들어서 관리한다

2. Find free frame
3. 디스크 오퍼레이션에게 스케줄링을 시켜서 디스크에서 해당 페이지 내용을 가져옴
4. valid-invalid bit를 v로 바꾸고 페이지를 업데이트
5. 메모리 요청을 한 인스트럭션을 restart



### Demand Paging의 특성

프로세스 시작할 때 메모리에 아무것도 없음

프로세스의 첫 번째 인스트럭션 자체가 메모리에 없음

처음부터 페이지 폴트 발생

모든 첫 번째 access에서는 page fault가 발생한다



하나의 인스트럭션이 여러 개의 메모리에 접근을 요청한다면?

* multiple page faults 발생
* 로컬리티때문에 자주 발생하진않음



### Instruction Restart

> 만약 b = ++a; 라는 코드를 처음 실행할 때
>
> a와 b가 다른 페이지에 있어 page fault가 두 번 발생한다면 
>
> 1. a를 읽어옴 <- page fault
> 2. a에 1을 더해서 a에 저장
> 3. b에 a값을 저장 <- page fault
>
> 2까지 하고 b를 위해 page fault를 하고 다시 2다음부터 수행한다?
>
> * 어려움. 하나의 instruction이므로 atomic 해야 함
> * 아예 재시작 : 2번에서 a값 늘렸는데 어떻게... 되돌리죠?

그럼 인스트럭션 중간에 page fault가 발생한다면 어떻게 해야할까?

* auto increment/decrement : 하드웨어 서포트가 필요
  1. instruction시작전에 page fault 검사
  2. 수행하다가 page fault 발생하면 해당 instruction이 바꾼 내용을 전부 원상복구



## TLB Fault

모든 페이지 펄트는 티엘비 펄트를 항상 유발 (당연히 TLB에도 정보 없음)

Page Fault가 나면?

1. MMU가 OS에게 trap걸어줌
2. OS는 기존에 수행하던 유저 레지스터와 프로세스 state를 저장함
3. 방금 온게 page fault였는지를 다시 판단함
4. Invalid인지 단순 mem에 없는건지 확인
5. 디스크의 어디에 이 페이지가 있는지 찾는다
   * 아예 처음시작이에 프로그램 파일에 적힌 코드나 데이터면 swap space에도 없음. 1. program file에 있음
   * 한 번 불렸다가 쫓겨난 애거나 빈 스택이면 2. swap space
6. free frame에 가져옴 (디스크한테 요청보냄 I/O)
7. I/O를 기다리는동안엔 cpu에서 다른 프로세스가 실행될수도있음
8. I/O interrupt를 받으면 돌던 프로세스의 레지스터와 스테이트를 저장
9. 인터럽트가 디스크에서 온건지 확인
10. 페이지 테이블을 수정하고 메모리에 있다고 표시해줌
11. cpu가 해당 프로세스를 다시 시작할 수 있게 기다림
12. restart instruction





* 오래 기다려야함
* 퍼포먼스에 안좋은 영향을 끼칠수도
* 이미 page에 있는애 쫓아낼 때도 I/O발생
* 다시 시작하려면 레지스터그런거 컨텍스트 스위칭

EAT 

> TLB때보다 더 큼 

* 프로세스를 바꾸고 다시 넣고하는 비용. 레지스터 대피 비용. state저장 비용 이런거 다 합친게 page fault overhead
* swap page out : free frame이 없으면 victim page가 swap space에 다시 써지는데 걸리는 시간
* swap page in : 사용하고 싶은 page가 disk로부터 읽어 들어와지는 시간



### Key Issues

* Page selection
  * 언제 어떤 페이지를 가져올까?

* Page replacement
  * 언제 어떤 페이지를 내보낼까?
* Page frame allocation
  * 각 프로세스에게 프레임을 얼마나 할당해줄까?



### Page Selection

Page Selection Policies

* Demand paging
  * 시작할 때 아무것도 없고 page fault가 일어나면 그 때 가져옴
* Prepaging
  * HDD에서 한 번 가져올 때 인접한 공간에 있는 것도 같이 가져오자
  * 인접할 경우도 ... 많은 건 아님? 예언자가 있어야...

* Request paging
  * 직접적으로 요청
  * 유저가 직접 나 이 페이지 필요하니까 미리 읽어줘~
  * 어려움. 

그냥... 디맨드 페이징 사용함



### Copy-on-write

첫 프로세스를 제외하고는 기존 프로세스를 복제

새로 만드는게 아니라 기존의 것을 재활용하자

실질적으로 데이터를 바꿔쓰는 write은 데이터를 바꿀때 시작됨

fork : 똑같은 페이지지만 별도의 영역에 복사

vfork : 프레임에 있는 같은 페이지를 가리키게 함

* 동일한 pagetable

요즘은 그냥 fort써도 vfork처럼 동작;;;



## Page Selection

* exec()하거나 코드가 달라져서 데이터를 수정하게 되면 그제서야 실제 page fault일으키고 데이터를 가져와서 프레임에 채운다



## Page Replacement

EAT를 줄일 수 ㅇㅆ는 가장 중요한 요소

프레임이 꽉 차있다면 누군가를 아웃시켜야함

만약 A가 변경됐다면 victim frame을 반드시 hdd에 write해줘야함

* 변경이 안됐으면 어차피 hdd에 저장되어있으니까 그냥 삭제만 하면됨

EAT를 줄이는 방법

1. victim을 고를 때 non-dirty 를 swap out( 다시 write안해도 됨)
2. 다시 swap-in 될 page는 가급적으로 swap-out에서 배제
