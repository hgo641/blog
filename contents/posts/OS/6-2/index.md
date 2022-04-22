---
title: "os 6-2"
date: 2022-04-06
tags:
  - OS
---

큐마다 다른 폴리시 - 멀티레벨 큐



어떤 프로세스가 cpu바운드인지 io바운드인지 알 수 있?

그냥 프로세스만 받아서는 알기 어려움



멀티레벨큐 : 사용어렵 잘 설계해야함

멀티레벨피드백큐

여러 개의 큐를 두고 각각의 큐에 대해서 서로 다른 스케줄링 알고리즘을 가져감

프로세스가 여러가지 큐를 옮겨 다닐 수 있음



어떻게 옮겨가는가? 사용자가 결정해야함

피드백 방법에 근거해서 결정함. 이전 스케줄링의 결과가 다음 큐를 결정한다

한 프로세스가 특정 큐에 종속되는게아니라 계속 옮겨다닐 수 있음

ex) 네개큐. 라운드로빈. 타임퀀텀만 다르게가져감

타임퀀텀이 짧은건 I/O 바운드 어플리케이션에게 유리함. 긴건 cpu 바운드

- 처음에는 다 Q0으로 들어감(프로세스가 어떤 애인지 모름)

- 만약 프로세스가 주어진 타임 퀀텀(타임 슬라이스)을 다 쓴다면 프라이어리티를 하나 낮춘다 : 타임퀀텀이 부족해서 끝남 -> Q1으로 내려줌.
- 블로킹되어서 끝났다면. 아이오요청을하면서 기다려야해서 waiting queue로 내려옴. 반대로 increase. 프라이어리티를 높인다(아이오바운드일 확률)

동일한 나이스 밸류를 가지면(동일한 타임퀀텀)



##  Linux Scheduling

### Completely Fair Scheduler (CFS)

큐레벨 말고도 스케줄링 클래스라는게 존재

#### Scheduling classes

두 개의 클래스가 존재

* default
* real-time : 주어진 시간내에 반드시 그 일을 끝내야하는

real-time이 default보다 큰 우선순위를 갖는다

quantum based(고정된 시간)가 아닌 proportion(각각의 프로세스에게 전체시간중에 얼만큼을 할당해줄지. 비율을 어떻게 할당해줄지)

*  Variable quantum size



#### nice value

default class에서 -20 + 19

낮은 숫자가 높은 우선순위



#### virtul run time (상대적인 실행 시간)

시스템은 가장 낮은 virtual run time을 선택

> 동일한 nice value를 가지는 두 개의 프로세스 존재(p1, p2)
>
> nice value가 같으면 할당받을 수 있는 time quantum도 동일(20ms라고 가정)
>
> p1 (20) - p2 (20) - p1(20)
>
> 현재 p1(40), p2(20)만큼 돌았음
>
> 이 때 둘의 nice value가 같기 때문에 p2선택

​		

> 서로 다른 nice value를 가지는 두 개의 프로세스 존재
>
> p1 weight : p2 weight = 3:1
>
> time quantum은 동일 (nice value가 달라도 time quantum은 같을 수 있다)
>
> p1 (20) - p2 (20) - p1(20)
>
> * weight 계산된 적적 cpu time 비율 (p1-45, p2-15)
> * 실제 차지한 cpu time ( p1(40), p2(20) )
>
> 
>
> scale factor (weight의 역수)
>
> p1 scale factor = 1
>
> p2 scale factor = 3
>
> virtual runtime = 실제 cpu time x scalefactor
>
> p1의 virtual runtime은 40 * 1 = 40
>
> p2의 virtual runtime은 20 * 3 = 60
>
> virtual runtime이 적은 p1을 고른다



* default의 nice value는 (-20~19)
* real-time 까지 합쳐서 보면 -20 = 100, 19 = 139
