---
title: "os 10-1"
date: 2022-05-02
tags:
  - OS
---

## Mutex Lock

> * race condition - 문제 상황
> * critical section - 문제 정의
> * mutex, semaphore, monitor - 해결방법 (high-level) API : OS가 제공해줘야함

Mutex (Mutual Exclusion) : 공유 변수 + 공유 자원(HW자원, I/O device )

ex)

> * 프린터룸 : critical section
>
> * 프린터기 : 공유 자원
>
> * admin : os - key를 관리한다
> * 각각의 사람들 : process or thread
> * key : mutex
>
> 
>
> os는 mutex권한을 acquire하거나 realease함
>
> * Critical section을 보호하는 기법
> * acquire()과 release()를 사용해서 섹션에 진입하는 것을 제어한다. 인터페이스 제공
> * Boolean variable == mutex
> * key를 acquire하거나 release한다



#### acquire() - key를 전달

* admin이 수행

- key가 없으면 대기
- key가 생기면 acquire()이후의 코드를 실행 (크리티컬 섹션으로 진입)



#### release() - key를 반납

- 할 일이 끝났으니 available을 true로 만듦



#### Busy waiting

>  key를 얻기전에 while문을 계속 도는 것

- available이 참인지 거짓인지 계속 판단
- while을 깨줄 수 있는건 외부의 다른 프로세스
- P0 : time slice를 소진할 때 까지 busy waiting
- availble을 참으로 바꿔주는 건 다른 프로세스 : 다른 프로세스가 cpu에 올라가서 참으로 만들어줘야하나 이미 P0이 cpu 선점
- P0은 계속 while문을 돌리면서 cpu에 있어야함
- 성능 손해
- spinlock == lock을 위해서 계속 spin중이다



## Semaphore

mutex와 비슷하나 좀 더 진화된 형태

key가 여러개

- P() : wait, (=acquire)
- V() : signal, (= release)
- S : 몇 개의 key인지 - 0보다 큰 int (S가 1이면 mutex와 같은 기능. mutex에서는 0과 1뿐)
- Binary semaphore : S가 0또는 1 (mutex와 같은 기능)
  - for 전통적인 공유 변수 제어
- Counting Semaphore : S가 2보다 크거나 같음
  - for   공동의 리소스 자원 관리/제어
  - wait()과 signal() 사용 - 스케줄링 필요
  - 단순히 공유 자원의 lock개념을 넘어 확장됨
- wait()

- signal()

- wait()과 signail()(P, V)을 수행하는 것도 크리티컬 섹션에 포함시켜야한다

  - busywaiting 발생

- busywaiting해결을 위해 세마포어 전용 waiting queue를 만든다

  - block == sleep : wait을 호출한 프로세스를 waiting queue에 넣는다
  - wakeup : waiting queue에 있는 애를 제거하고 cpu의 ready queue에 넣는다
  - value랑 list도 크리티컬하게 관리되어야함 -> 싱글 cpu일 경우, disable interrupt, inable interrupt 사용
  - 앞에서는 disable interrupt하는거 오버헤드 크다고 했는데 여기선 쓰는 이유 : disable해두는 시간이 짧음
  - 멀티 cpu - 멀티 프로세서 : turn off all other processors : 멀티 cpu일때는 위처럼 다른 cpu의 interrupt도 막는 것은 위험. atomic hardware support가 필요(test_and_set) - S와는 별도로 critical section 해결을 위한 boolean 변수 필요 - 복잡

  
