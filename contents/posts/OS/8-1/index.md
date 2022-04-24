---
title: "os 8-1"
date: 2022-04-22
tags:
  - OS
---



## Critical Section Problem

> 동기화!
>
> 각 프로세스의 코드에는 크리티컬 섹션이 존재한다
>
> * critical section : **공유 데이터**가 업데이트 일어나는 부분
> * entry section : 크리티컬 섹션에 들어가기위한 코드의 부분
> * exit section : 크리티컬 섹션에서 빠져나오기 위한 코드의 부분
> * remainder section : 크리티컬섹션이 아닌 나머지부분
>
> 위 섹션들은 모두 코드임



* n개의 프로세스가 있다고 가정

* 각각의 프로세스는 코드를 가지는데 그 코드에 크리티컬 섹션이 존재

* 크리티컬 섹션안에서는 정보의 업데이트가 가능하다

* 공유되는 변수나 테이블등을 바꿀 수 있음

* 어떤 프로세스가 크리티컬 섹션을 가지고있으면 다른 프로세스가 그 크리티컬 섹션에 진입할 수 없다

  

#### Solution to Critical Section Problem

* Mutual Exclusion
  * 한 번에 한 프로세스만 크리티컬 섹션에 들어갈 수 있다
  * 한 프로세스가 어떤 크리티컬 섹션에 들어가 있으면 다른 프로세스가 동일한 크리티컬 섹션안에 들어가지못한다

* Progress
  * 만약에 크리티컬 섹션에 아무도 없는데 들어가고싶어하는 사람이 있으면 그 결정은 바로 나야한다
  * 즉 최대한 빨리 결정이 나야한다는 것! 무기한 연장되지않는다
  * 그 결정은 remainder 섹션에 있지 않은 애들이어야한다 
* Bounded Waiting
  * 어떤 프로세스가 크리티컬 섹션에 들어가고 싶어서 request를 날렸을 때 바운디드 웨이팅에 한계가 있다. 즉 무기한 기다리지않는다



#### Critical Section Handling in OS

> **예시** : 크리티컬 섹션 핸들링이 되지않는다면?
>
> fork()를 하면 next_available_pid를 제공
>
> 만약 두 프로세스가 동시에 fork()를 한다면?
>
> 동시에 next_available_pid에 접근해서 
>
> 방금 생성한 프로세스 p2, p3가 서로 다른 프로세스임에도 같은 프로세스 아이디를 가져갈 수도 있다



* preemptive(cpu를 뺏어갈수있는 스케줄링)
  * 어떤 프로세스가 커널모드에서 run하고 있을 때 cpu를 뺏는게 가능
  
* non-preemptive (능동적)
  * cpu 뺏는게 안됨
  * 특정 프로세스가 커널모드를 끝날 때까지 or 양보할 때까지 동작하는게 보장이 됨
  * 커널 안에서의 작업이 **완전히 끝나는 것을 보장**
  * 애초에 critical problem, race conditions가 발생하지않음
  * 일을 하는중에 누군가 뺏어가지않음
  * **문제가 생기는건 preemptive한 커널 구조**
  
  

어떤식으로 해결해야할까?

### Peterson's Solution

* 고전적인 소프트웨어 기반 해결책. 현대에는 적용할 수 없음
* 두 개의 프로세스가 동시에 크리티컬 섹션에 들어갈때(두 개의 프로세스일때만 가능. 여러개는 못함)
* 로드, 스토어 명령어는 atomic하다고 가정. = 명령어 중간에 껴들어갈수없다
* 싱글 cpu가정
* 두가지변수사용
  * int turn : 누구 순서인지
  * bool flag[2] : 
    * flag[i] == True이면 Pi가 크리티컬 섹션에 들어갈 준비가 되었다는 뜻
  * turn = j; #양보함 -> turn이 j로 바뀌어서 while문을 빠져나옴?
  * pj가 다 끝나면 flag i를 다시 false로 바꿔줌??
  * (while flag[j] && turn == j ) : 기다림
  * i가 크리티컬 섹션에 들어갈 수 있을 때는
  * 1. 상대방이 준비가 안됐을때
    2. 나의 턴일 때

> while문과 critical section이 별개인듯
>
> 먼저 순서를 양보해서 j가 준비되었고 턴이 j면 i는 while문을 계속 돎
>
> j의 크리티컬 섹션이 끝나서 flag[j] = false가 되면 i가 while문을 빠져나오고 크리티컬 섹션에 진입



그러나 현대 컴퓨터에서 쓰일 수 없음...

가장 큰 이유는 reorder때문

아무리 순차적으로 코드를 짰어도 그거에 디펜던시가 없으면 컴퓨터가 효율성을 위해서 리오더링할 수 있음

cpu가 알아서 turn을 먼저하고 flag를 나중에 해버림



#### Critical Section with Disable/Enable Interrupt

어떻게 막을 수 있을까?

누군가 크리티컬 섹션에 진입했을때 인터럽트를 막아버림

인터럽트 : 컨텍스트 스위칭을 유발할 때 생김

컨텍스트 스위칭을 하기위해 인터럽트를 보냄

인터럽트를 막았기 때문에 컨텍스트 스위칭이 되지않음(p1에서  p2로 넘어가지않음)

* 그러나 인터럽트를 disable/enable 하는 것은 너무 강력한 메소드!

* 이거 하려고... 인터럽트... 다 막으려고...~?
* 벼룩잡으려다 초가삼간 태우는 꼴!

> 소프트웨어만으로는 해결하기 힘든 문제
>
> 하드웨어의 도움이 필요하다!

## Synchronization Hardware

### Hardware Instruction : test_and_set

* atomic = non-interruptible한 instruction

* 어떤 메모리에서 테스트 값을 가져와서 그 값을 바꾸는 일도 함



boolean test_and_set (boolean \*target)

> target을 받아서 값을 가져옴
>
> 타겟을 true로 만듦
>
> 받은 target값을 그대로 return
>
> \# test : 얘가 원래 true였는지 false였는지 테스트
>
> \# set : target을 true로 셋~ 



> 예시
>
> lock = target, lock은 공유변수
>
> 초기 lock값은 false
>
> tas()의 리턴값이 false라서 while문을 빠져나오지만
>
> lock이 true 로 set되어서 다른애들은 크리티컬 섹션 들어가지못함
