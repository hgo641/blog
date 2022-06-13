---
title: "os 14-2"
date: 2022-06-12
tags:
  - OS
---

## Page Replacement 

어떤 페이지를 내쫓을 것인가

reference string : 페이지가 access 되는 순서대로 페이지를 나열해둔 string

* Random : 생각보다 잘 동작하지만 optimal은 아님
* FIFO : 들어온지 가장 오래된 frame을 내쫓음
* Optimal : 미래를 예측해서 앞으로 가장 참조되지 않을 것 같은 페이지를 내쫓음 - 예측이 어려움
* LRU : 가장 오랫동안 사용되지않은 페이지를 내쫓음, 가장 optimal에 가까움



### FIFO

Belady's Anomaly

* frame을 늘렸는데 page fault가 더 많이 일어나는 현상



### Optimal

사실상 쓰이진않고 알고리즘을 비교할 때 사용됨



### LRU

Belady's Anomaly가 발생하지않음

* stack algorithm를 항상 만족하므로 증명 (Belady's Anomaly와 반대)
* n개 프레임이 있는 메모리가 있을 때 n프레임을 사용하는 페이지 set들은 항상 n+1프레임을 사용하는 페이지의 subset이다
* 왜?
  * LRU는 내쫓는 순서가 항상 똑같다?
  * n이든 n+1이든 항상 참조된지 오래된 애를 내쫓음
  * 참조되는 순서도 똑같으니 내쫓기는 순서도 똑같다 (n+1이면 그냥 한 박자 늦게 쫓겨나는 것일뿐...)
  * fifo는 쫓겨나는 순서가 다시 들어오는 순서에 영향을 미치기 때문에 항상 일치하지않는다

LRU의 단점 : Implementation이 어렵다

SW적인 해결방법

* Counter implementation
  * 언제 참조됐는지를 기록
  * 모든 페이지의 entry에 counter를 두고 참조될때마다 시간을 기록한다
  * 단점 : 페이지를 내쫓을 때 모든 페이지들의 참조시간을 비교해봐야하므로 다 뒤져봐야함
  * 페이지 엄청 많은데 언제... 다 뒤져?
  * 실행 어려움
* Stack implementation
  * 페이지가 참조되는 순서대로 linked list를 만듦
  * search가 필요하지않음
  * update 비용이 큼 (포인터 끊고... 옮기고)
  * 오버헤드가 큼
  * linked list 하드웨어로 구현하기 힘듦



## LRU Approximations

위처럼 구현이 어려워서 LRU를 완벽하게 구현하지않고 비스무리하게 만든다

* 하드웨어 support를 받음
  * reference bit(1bit)
  * 처음 page가 access되면 0
  * page가 다시 access되면 1
  * 0인 애들부터 내쫓음

1bit로 판단하기에는 부족

더 발전된 방법들은 아래에



### Additional-Reference

reference bit + 8bit register사용

* 각 페이지마다 9bit의 data를 가지고있음

주기적으로 [r-bit + 8bit]를 오른쪽으로 shift한다

* r-bit에는 0을 채운다
* 1이 앞에 있을수록 최근에 접근됨
* 1이 제일 뒤에 등장하는 애를 교체함

Time interval 형태로 LRU를 체크하는 형태

* 동작 자체에는 오버헤드 없음
* 0을 1로 바꾸고 shift하기만 함

단점

* 모든 페이지들의 bit를 비교해야함
* 모든 페이지마다 9bit씩 할당해야해서 오버헤드 발생 가능



### Second-Chance Algorithm

clock algorithm이라고도 부름

r-bit 하나 쓰는건 동일하나 FIFO와 결합됨

* 참조된지 가장 오래된 page를 찾음 : circular linked list이기 때문에 바로 찾음
* 해당 page의 r-bit을 찾음
  * 0이면 바로 교체
  * 1이면 second chance를 줌 : r-bit를 0으로 바꾸고 다음 순서를 탐색

* low overhead
* 1bit만 사용해서 여전히 불안정
* 모든 페이지의 r-bit이 1이면 fifo와 동일해짐 : 0을 만날 때까지 한 바퀴 돌아야 함





### Enhanced Clock Algorithm

* 탐색의 비용을 줄이는 방법은 아니고 정확도를 높이는 방법

> victim을 고를 때 
>
> * 미래에 있을 page fault를 줄이는 것이 중요
> * 수정이 된 애면 swap을 할 때 disk에 기록해야함
> * 수정이 된 애가 아니면 disk에 기록할 필요가 없음
> * 즉 수정이 안된 애를 내쫓는게 이득

(reference bit,  dirty bit)

교체 순위순

1. (0, 0) : 교체 일순위

2. (0, 1) : 최근에 access되진않았지만 modify는 된 애
3. (1, 0) : 최근에 access되어서 금방 다시 사용될지도
4. (1, 1) : 최근에 access까지 됐고 수정해서 교체 비용도 큼



(0, 0)을 만날때까지 탐색...

(0, 0)이 없으면 다음 순위인 (0,1)

최악의 경우에는 여러번 circular queue를 돌아야함



### Counting-Based Algorithm

잘 사용되지않음

각 페이지마다 counter를 줌

아까 걔는 시간을 기록하고 얘는 페이지가 access됐던 횟수를 기록

두 가지 알고리즘을 적용할 수 있음 (둘 다 common한 방법은 아님, 실질적으로 사용되는건 clock algorithm정도)

* LFU (smalles count를 교체)
* MFU (max count를 교체)
  * smallest count는 메모리에 들어와서 오래되지않은애 . 최근에 참조됐으니까 재참조가 되지않을까 하는 가정에서 수행



## Page Frame Allocation

각각의 프로세스한테 프레임을 개별적으로 할당해줄지, shared 영역으로 보고 할당해줄 것인지

각각의 프로세스에게 얼마나 많은 프레임을 할당해줄 것 인가?



#### Fixed Allocation

* Equal allocation이라고도 부름
  * 각각의 프로세스한테 동일한 숫자의 frame을 제공한다
  * 독립된 영역이면서 동일하게 할당
  * 프로세스마다 사용하는 양이 다른데 동일하게 할당하는건 불공평

* Proportional allocation
  * 프로세스가 사용하는 size를 보고 그에 맞춰서 할당해줌



* Priority Allocation
  * 사이즈가 아니라 priorities를 보고 



#### Global allocation

* 공유해서 쓰자~
* 모든 프레임을 공동으로 사용
* 프로세스에게 딱히 딱딱 잘라서 나눠주지않음
* page fault를 많이 일으키는 하나가 있다면 다른애들도 성능에 영향을 받을 수 있음
* 그러나 공유를 해서 효율성 증가 : throughput증가
* 일반적으로 쓰이는 방법



#### Local allocation

* 각자 쓰자~





## Thrashing

프로세스가 많이 돌아가고 있어서 페이지 개수는 많은데 메모리양이 작다면 page fault가 많아짐

계속 page fault나서 부르고 교체하는걸 thrashing이라함

physical memory가 프로세스들의 logical memory보다 훨씬 부족해서 page fault가 지나치게 많이 발생하는 것을 의미

페이지를 swapping하는데 시간을 다 씀

degree of multiprogamming을 하면 cpu utilization이 높아지지만 너무 많은 프로세스를 한 번에 돌리면 thrashing발생 위험이 있다



* 디맨드 페이징이 효과가 있는 이유
  * 로컬리티가 있어서
  * 같은 페이지가 참조될 확률이 높기때문에 page fault가 덜 일어남

* 왜 thrashing이 발생?
  * 각 프로세스들의 로컬리티 부분의 합이 total memory(physical) size보다 커짐
  * 만약 global이 아니라 local allocation을 했다면 프로세스마다 각자 공간을 할당받기 때문에 좀 줄어들긴함 (근데 일반적으로 global 사용)



쓰레싱을 어떻게 막을까?



## Working-Set Model

working set : 로컬리티 영역 (각 프로세스마다 자주 사용되는 메모리 공간) , 주어진 시간동안 사용되는 페이지들의 집합

* working set window : 페이지가 참조되는 시간을 나타냄
  * ex) 10000 instruction 

* working set window가 커짐 : 로컬리티 영역도 커지고 working set size도 커짐
* working set window가 작아짐 : 로컬리티도 작아짐 - 담을 수 있는 페이지 수가 작아짐



D : 각 프로세스들의 working set size 합 

* 이게 frame의 요구량이 됨
* D > m(실제 메모리 크기)이면 Thrashing 발생
* D > m인 상황이 발생하면 프로세스중 하나를 죽이거나 완전히 swap out시
  켜서 공간을 확보한다
