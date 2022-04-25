---
title: "4-2"
date: 2022-04-21
tags:
  - OS
series: "OS"
---

## Dispatcher

os의 프로세스를 구동하는 부분의 가장 안쪽이다

> 디스패처의 역할!
>
> 어떤 프로세스를 돌리고
>
> 스탑, 상태 세이브
>
> 다른 프로세스의 스테이트를 로드해옴



* switching context

* switching to user mode : 디스패처가 호출되는 시점은 커널모드.(os가 수행중) 바꿔주는 역할을 다 하고 나면 새로운 프로세스로 돌아가야함(유저모드)

* 적절한 위치로 점프 (새로운 프로세스의 실행해야할 위치, load state로 점프)



디스패치는 사실 수동적



### 디스패처는 어떻게 컨트롤을 유지할 수 있을까?

>  cpu는 한 번에 하나의 일만 할 수 있다
>
> 유저 프로세스가 실행되고 있다는 말은 디스패처가 실행되고있지않다는 말
>
> 그럼 어떻게 디스패처를 컨트롤할까?

* 📌 non-preemptive way : 프로세스가 디스패처를 깨워줄거라고 믿음
* 📌 preemptive way : 디스패처가 일정시간마다 깨어나서 프로세스를 쫓아냄
* interrupt, traps and faults에 의해 호출됨
* ex. interrupt (하드웨어)
  * 터미널에서 문자입력
  * 디스크에서 읽어오는거 끝남 (I/O)
  * 타이머
* Traps and Faults(소프트웨어) - 프로세스가 os한테 요청
  * 시스템 콜 : process가 os한테 어떤일을 해달라고 요청
  * floating  : 계산불가능. os가 처리
  * page fault



nonpremmtive는 완벽하지않음. 프로세스가 자리 안내줄수도있음. 

그래서 preemptive가 필요

 

## Scheduling Policy

1. 프로세스 테이블을 스캔해서 돌 수 있는애를 앉힌다 
   * 시간이 오래 걸림
2. 레디큐의 제일 첫번째에 있는 프로세스를 실행
3. 레디큐에서 우선순위가 높은 것을 실행

* 우선순위를 결정하는게 policy
* 왜 디스패처가 하지않을까?
  * policy와 mechanism을 분리하기위해

 

## Context Switching

* 디스패처가 기존의 state를 save하고 restore하는 것을 context switch mechanism을 통해 구현한다
* 다음 프로세스가 건들여서 손상을 일으킬 것 같은 모든 것을 저장한다.... 걍 웬만하면 다 저장?
  * Program counter
  * Processor status word(condtion codes 등)
    * 프로세스가 사용하던 cpu상태 정보
  * 레지스터들
  * 메모리는 정리하지않아도 될까? (책장)
  * 메모리는 저장하기에 너무 크고 비용이 큼
  *  memory management part가 별도로 담당하기때문에 context switching에서는 고려하지않는다



## Context Switching Implementation

machine... 에 따라 다른 컨텍스트 스위칭 수행존재...

os마다 구현하는 방법에 따라 다름



status를 메모리에 옮김

메모리의 어디에? 스택 or PCB

* 스택 : 프로세스가 사용하는 메모리 공간
* PCB : os가 사용하는 메모리 공간



뒤에 예시 그림은 stack

* before interrupt

* interrupt call

  수행중이던 프로세스 멈추고 prcoess status word를 넣고 그 다음 return addr(=pc)를 넣음

* PUSHA instruction

  책상에 있는걸 다 책장에 정리해야함

​		레지스터 정보를 모두 스택에 저장

​		단 %rsp는 원래  return addr를 가리키던 n이었으나 레지스터 정보를 넣고 n-a로 업데이트된다(실제로는 n-a까지 들어갔지만 스택에는 n이 들어가버림) 

* Saving stack pointer

​		**그래서 %rsp는 os-PCB의 StkPtr에 저장**

* Selecting Next Process

  scheduling policy는 OSPCBCur가 새 프로세스 OS-PCB의 StkPtr을 가리키게함

  OS-PCB의 StkPtr값을 CPU's StkPtr에 넣음

* POPA instruction

  새 프로세스의 스택에 있는 레지스터들을 전부 가져옴

  그 다음 return addr과 PSW도 가져온다



Selecting Next Process과정에서 만약 새 프로세스가 처음 생성된 애, empty 스택이라면?

스택이 비었다면 디스패처 오류

시스템은 처음 생성된 프로세스에게 임시로 stack에 할당해줌 디폴트값



## CPU Scheduling

## Scheduling Policy

우선순위 기준점

* cpu utilization : 책상 사용량을 증대!
* throughput : 주어진 시간동안 몇 개의 프로세스가 일을 했냐
* turn around time : 특정프로세스가 일을 하고 싶은데 얼마나 자주? 보통 낮으면 좋지만 무조건적인건아님
* waiting time : 기다리는 시간
* response time : 프로세스한테 job을 요청했는데 job이 끝날때까지의 시간. 프로그램 종료를 뜻하는건 아님. 특정 동작



### First-Come, First-Served(FCFS)

fifo라고도 불림 first in first out

먼저 왔으면 먼저 내보냄

문제 : 앞에 애가 엄청 오래걸리는 애인데 뒤에 애들은 몇 초만에 끝날 때-monopolize, Convoy effect

해결방안 : 먼저 온 애를 먼저 앉히되, 특정 시간동안만 일하게하고 다음에 부름 - maximum time을 **timeslice**라고 함

time slice외에 I/O요청등에 의해 내려오는 경우도 있음. 이걸 blocked됐다고 말함



### Shortest Job First(SJF)

그럼 짧은 애부터 실행하자!ㅎㅎ

