---
title: "OS의 역할과 역사"
date: 2022-03-29
tags:
  - OS
series: "OS"
---

# OS(운영체제)란?
컴퓨터 시스템의 자원들을 효율적으로 관리하며 사용자가 컴퓨터를 편리하고, 효과적으로 사용할 수 있도록 환경을 제공하는 여러 프로그램의 모임.
사용자와 하드웨어간의 인터페이스로서 동작하는 시스템 소프트웨어의 일종으로, 다른 응용프로그램이 유용한 작업을 할 수 있도록 환경을 제공해줌.
ex) Windows, Linux

# OS의 역할

- resource allocator
- program controller

for 유저의 편리한 사용
for 좋은 성능

- 공유된 컴퓨터
  큰 서버의 shared resource를 어떻게 분배할 것인가
  handheld computers는 리소스가 작음. 배터리 처리
  임베디드 컴퓨터는 유저 인터페이스가 없는 경우가 있어 os가 처리

# OS의 역사
## 1. (Early 1950s - Mid 1960s)
하드웨어가 비쌈
goal : 어떻게 하드웨어를 효율적으로 사용할 것인가
punch card를 사용해 하나하나 코딩
사람이 os의 역할을 수행 (operator)

- 카드를 받고 출력을 해서 유저에게 전달
  <bold> slow job to job </bold>

---

###batch monitor(여러개의 job을 묶음)
저성능 컴퓨터, 고성능 컴퓨터 함께 사용
저성능은 인풋, 아웃풋
고성능은 연산
<bold> faster job to job </bold>

---

###batch monitor(최초의 os)
하드웨어 메커니즘이 나옴 - I/O와 CPU 분리
I/O를 하는동안 cpu연산을 못해서 분리

inturrupt : cpu에게 I/O작업이 끝났음을 알려줌

버퍼링과 interrupt 핸들링작업이 os에 추가됨

- computation과 비동기적(async) I/O가 분리됨

sync I/O - read : 읽어와야 다음 작업 가능
async I/O - write : 쓰는 행위는 다음 작업에 영향을 안끼침
(모든 read가 sync, write가 async인건 아니지만 대체로 그러함)

###Multi-programmed batch monitor

- 동시에 둘 이상의 active한 job(시작되었으나 아직 끝나지않은 작업)을 수행
- sync I/O도 분리
- Degree of multiprogramming >= 1 : active job을 얼마나 수행할 수 있는지

####멀티 프로그래밍이 생기면서 시작된 문제들
\*Memory protection and relocation
하나의 job이 실수로 다른 job에 영향을 끼침
다른 job의 addr를 침범
하나의 메모리공간에 여러 job이 올라감. job의 주소 공간 예측 불가능 - 로지컬 주소와 피지컬 주소의 구분이 생김
-> MMU(Memory-Management-Unit)

\*Higher utilization

\*Concurrent programming
하나의 자원을 공유하면서 생기는 문제

##2. (Mid 1960s - Mid 1990s)
하드웨어가 저렴해짐, 인력이 비싸짐
goal: 인력을... 효율적으로 ...

###Interactive time-sharing OS
하나의 cpu를 분할해서 여러 사람들이 사용

- 터미널이 저렴 : 메인컴퓨터 하나에 여러 터미널 존재
- 파일 시스템 등장
- Response time과 protection이 더욱 중요해짐

###PC OS - personal computer
각자 컴퓨터가 생김
OS가 subroutine libaray로 나눠짐(OS를 큰 기능별로 나눠서 사용)

##3.(Mid 1990s - )

- 인터넷
  Network I/O가 생김
- 동영상등 멀티미디어 서포트
  QoS(Quality of Service)
  -> OS scheduling 발전
  예전 : priority - 우선순위 기반
  멀티미디어 서포트 이후 : badwidth 기반 - 각각의 프로세스에게 조금씩 할당
- connected multimedia services
  스트리밍
- 모바일
  적은 리소스, 배터리를 효율적으로 관리
- 클라우드 컴퓨팅
  virtualzation

#OS의 특징

- Large
  수천만 라인
- Complex
  비동기적인 behaviors의 집합체
  하드웨어 특성의 이해 필요
- Poorly understood
  오랜시간동안 기능이 추가되며 만들어짐
  코드에 대한 이해도가 떨어져감
