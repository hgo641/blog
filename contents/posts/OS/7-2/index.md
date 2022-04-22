---
title: "os 7-2"
date: 2022-04-22
tags:
  - OS
---



### Combined Threads

* many to many

* 커널레벨모드도 유저레벨모드도 존재

* 유저가 나눠놓은 일의 단위와 cpu가 할당한 주체의 단위를 다르게 가져간다

* 커널은 사실상 스레드의 존재를 크게 의식하지않음

* 커널은 단지 유저에게 virtual processor같은. cpu코어같음

* 커널은 유저가 요청한대로 커널레벨스레드를 만들어주고 커널레벨스레드단위로 스케줄링하고 관리해줌

* 유저레벨 스레드는 커널레벨 스레드가 가상의 프로세서라고 생각하고 활용

* 어떤 스레드를 어떤 프로세서에 넣을지 관리를 해줌

* 사용자 : 자기 자신이 스레드를 만들고 관리
* 시스템 : 시스템도 똑같이 만들고 관리
* 시스템(커널) 스레드 수의 한계가 존재(1대1로 매핑하면 오버헤드가 커짐)
* 유저 레벨 스레드 > 커널 레벨 스레드
* 유저 레벨 스레드 라이브러리
  * 유절벨스레드를 만들고 없앰
  * 스케줄링 ( 어떤 스레드를 선택할지, 어떤 유저레벨스레드를 커널레벨스레드에 올릴지 )
  * 컨텍스트를 saving, restore
  * 스레드간 데이터, 메시지 통신

* System call API and kernel functions for thread facility
  * 커널레벨 스레드 중에 어떤걸 cpu에 올릴지라는것만 빼고 다 동일



* 유저가 스레드를 만듦
* 커널에서는 유저 스레드의 양을 보고 적절한 양의 커널 스레드를 만들어서 매핑
* 유저레벨 스레드간의 소통, 싱크로나이즈는 유저레벨에서 이루어짐
* 사실 요즘 하드웨어는 cpu코어가 적당히 많아서 원투원 사용



## Thread Libraries

* Pthread & OpenMP
* Pthread(Unix 계열의 표준 API)
* OpenMP (c++, c)

### Pthreads

* Pthread : POSIX standard for threads
* api만 정의되어있음
* 구현은 실제 라이브러리를 만드는 사람마다 다르게 사용할 수 있음



* pthread_create()는 fork()와 비슷한
* pthread_exit = exit()
* pthread_join() = wait
* pthread_yield() : cpu를 양보하겠다는 말. 다른 스레드가 써라!



pthread_create(&tid, NULL, runner(수행할 일), argv\[1](runner함수의 인자) )

스레드 attribute

​	ex. 스케줄러, 폴리시, 프라이어티, stack주소등



## Implicit Threading

* explicit threads (명시적으로 스레드 생성, 일 분담) - 어렵다 적당히 시스템이 해줬음좋겠다라는 마음에서 나온게 implicit threading
* 스레드 생성, 관리를 컴파일러나 런타임라이브러리가 해줌
* 프로그래머는 단지 병렬적으로 일할 수 있는 부분만 알려줌
* 사용자수준에서는 훨씬 사용하기쉽다

### OpenMP

컴파일러한테 여기 parallel하게 해줘~하면 알아서 병렬처리 해주는 api와 compiler directives

* #pragma omp parallel
* 앞에 이 문장만 넣어주면 병렬로 해줌 밑에 나오는 줄을 코어 수만큼 수행. 병렬로 할 수 있는만큼 중복수행해줌
* #pragma omp parallel for (num_thread(4))
* for문의 크기를 적당히 쪼개서 병렬로 처리해줌
* num_thread를 통해 thread를 몇 개 쓸건지 정할수도있음



## Synchronization Tools

프로세스들간의 소통이 필요

data inconsistency를 야기할수도

a라는 애가 읽으려는 데이터를 b가 읽기전에 바꿔버리면 예측하려는 결과가 안나옴

counter로 버퍼사이즈를 카운트

atomic하지않음 - Race Condition - 자원을 공유해서 잘못된 결과를 낳음

그래서 동기화가 필요!

프로듀서가 쓸거라고하면 컨슈머가 건들지못하고 컨슈머가 쓸려고하면 프로듀서가 쓰지못하는
