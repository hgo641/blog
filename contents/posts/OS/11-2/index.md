---
title: "os 11-2"
date: 2022-05-11
tags:
  - OS
---

#### mutual exclusion을 방지하려면?

현실적이지않음

mutual exclusion이 필요해서 하는건데...

#### no preemption

이것도 현실적으로 쉽지않음

#### no hold and wait

원래는 다른애가 가지고 있으면 무한정 기다려야하는데

한꺼번에 획득하면 기다릴일이 없음

아예 가져가거나 못가져가거나

atomic한듯

#### no circular wait

circular wait를 만들지않으려면 모든 리소스에 번호를 부여

리소스를 할당받을 때 낮은 번호의 리소스부터 할당받게한다

누군가가 높은 번호를 가진 상태에서 낮은 번호를 요구할 일이 없음. 모든 리소스 할당이 낮은 순서부터되기때문에 아예 할당을 못받거나 순차적으로 할당을 받거나

자연스럽게 circular wait이 해결된다

모든 프로세스들은 순서에 맞춰서 리소스를 할당받는다 근데 사실상 이것도 어려움

1. 몇 개의 리소스가 있는지도 모름

2. 시스템마다 리소스 수가 다를 것

3. os가 일정한 order를 가지고 순서를 부여학 쉽지않음

4. 새로운 리소스가 추가되면 어떻게 할 것인지

   단순하게 i+1?

   근데 얘가 굉장히 중요한 리소스라면?

이론적으로는 괜찮으나 실질적으로 어렵다

## Deadlock Avoidance -

시스템이 추가적인 정보 요구 : a prior information

사전 정보 필요

- maximun number of resource : 그들이 동시에? 사용하고자하는 최대 리소스의 개수를 미리 개재

-살짝놓침-

### Banker's Algorithm

시스템은 state를 safe state와 unsage state로 나눔

- Safe state : 어떤한 형태에도 데드락이 발생안함

- Unsafe state : 데드락이 발생할수도있음

P : 프로세스 n개의 set

R : 리소스 m개의 set

safe state인지를 어떻게 볼까

#### 전제조건

- 각각의 프로세스는 사전에 maximum use를 고지해야함

- 프로세스는 기다릴 수 있어야함

- 프로세스가 한 번 리소스를 가져가면 주어진 시간안에 반환을 해야함

#### Notation

- n : 프로세스의 개수
- m : 리소스의 개수
- Available[1:m] 리소스마다 몇 개가 사용 가능한지
- Max[1:n, 1:m] : n x m 행렬, 각 프로세스가 최대로 필요로 하는 리소스 개수
- Allocation[1:n, 1:m] : 현재 각 프로세스들에게 할당된 리소스
- Need[1:n, 1:m] : 각 프로세스들에게 필요한 리소스들 (Max - Allocation)

알고리즘 두 가지

- safety algorithm : 현재 safe인지 unsafe인지 판별
- Resource-Request Algorithm : 새로운 리퀘스트가 왔을 때, 이 리퀘스트를 들어주면 safe인지 unsafe인지 판별 (safety algorithm 사용)

### Safety Algorithm

Availabe보다 Need가 더 작은애들은 언제든지 일을 할 수 있음

P1을 먼저 선택

이전 : 2 0 0

추가 : 1 2 2

Avaiable + 이전꺼 반납 (2개 반납)

Finish : 해당 프로세스가 끝났는지

Available == Work

캬아악! 헷갈려

### Resource Request Algorithm

리퀘스트를 받았다고 가정하고 safety algorithm을 돌려봄

### 한계

1. Rj가 얼마나 될지 정확하게 알기 힘듦
2. Process 수가 가변적임
3. 미리 maximum use를 고지하기 힘듦

그래서 사용되진않음 ㅎ

사실 데드락을 크게 고려안함... 왜?!

데드락이 발생하면 pc사용자가 바로 인지할 수 있음

무한뤂프 돌면서 뻗을때... 걍 작업관리자 켜서 종료함

오잉~ 나 스스로 데드락 종료 ?!

그래서 엄청나게 중요한 문제는 아님

유저가 개입할 수 없는 서버시스템, misstion critical system(자율자동차)...에서는 중요한 문제

prevention detection 차이

prevention : 비쌈, 비효율적. 데드락 가능성을 보고 아예 방지

detection : 데드락을 허용하되 리커버리 제공

근데 사실 디텍션도 그렇게 값싼건아님

복구가 항상 가능한 것은 아님

mission critical system에서 사용이 어렵?

(그럼 뭔 쓸모냐?)

## Deadlock Detection

- single instance
- multi
