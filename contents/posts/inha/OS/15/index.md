---
title: "os 14-2"
date: 2022-06-12
tags:
  - OS
---

## Working-Set Model

어떤 프로세스의 워킹셋은 어떻게 구할까?

reference bit을 사용한다

* 페이지가 접근될때마다 r-bit을 1로 바꿔줌
* 1인 애들을 모아 워킹셋을 만듦
* t1이 지나면 1을 0으로 초기화

워킹셋을 델타시간때마다 구할 수 있다

델타가 커지면 워킹셋을 구하기 애매

ex)

10000일 때 5000time으로 나누고

각 페이지마다 r-bit 2bit준비

페이지가 access되면 1st rbit에 1을 넣음

interrupt가 들어오면 1st rbit을 2nd rbit에 옮겨 닮고 1st를 초기화

interrupt이후 access된 메모리의 1st를 1로 만듦

1st와 2nd에 있는 애들을 워킹셋에 추가함

조금 더 작은 단위로 조사할 수 있다...



더 자세하게 하고싶으면 더 세분화로 나눔... + r  bit많이 추가.

단, 페이지마다 rbit을 관리해줘야해서 비용이 커짐



## Page-Fault Frequency

허용 가능한 수준의 PFF(page fault frequency)를 사전에 정함

local replacement policy를 통해서 프로세스마다 할당하는 프레임을 변경시킨다

* actual rate가 낮으면 frame수를 낮춤
* actual page fault rate가 크면 frame수를 높임

-> PFF가 적절하게 조절될 수 있음

단점 : 프로세스가 많아지면 모든 프로세스가 가져갈 수 있는 프레임수가 줄어든다



사실 가장 근본적인 해결책은 메인 메모리를 많이 꽂아주는거

그러나 메인 메모리도 늘어나는데 한계가 있다

리눅스의 경우, pagefault가 많이 일어나면 특정 프로세스를 죽임 - OOM killer

* 가장 많은 공간을 얻을 수 있는애를 죽임
* OOM_SCORE : 높을수록 메모리 회수를 많이 할 수 있음



## Allocating Kernel Memory

커널 메모리는 미리 메모리 풀을 할당해놓음

왜 커널은 따로 관리?

커널 메모리 공간중에 일부는 연속적일 필요가 있다.

ex) device io를 위한 메모리공간

일반적으로 커널은 메모리allocation함수를 자주 호출한다

 allocation할 때마다 internal fregmentation이 많이 발생할수도 

그래서 커널은 미리 많이 떼어두고 그 안에서 작게 작게 잘라서 준다



어떻게 잘라서 줄까?

### Buddy System Allocator

커널을 위해서 연속된 공간을 할당받아둠

미리 할당 받은 공간을 이진 바이너리 처럼 관리

특정한 함수가 공간을 할당해달라고 하면 그 공간의 크기가 될때까지 계속 반으로 나눔

ex) 총 256KB인데 32KB 공간을 할당해달라는 요청이오면, 256 -> 128 2개 -> ... 32가 나올때까지 반으로 나눈다 (나머지는 free영역으로 관리)



다음에 어떤 메모리 요청이 오면 그거에 맞는 공간을 할당해주기 쉬움 (여러 크기로 나눠져있는 상태)

세분화 & 합치는게 빠름



internal fragmentation이 심할수도

* 아무리 잘게 쪼개도 2의 제곱 형태



메모리를 항상 2의 제곱형태로 관리





### Slab Allocator

커널은 하나의 큰 메모리 공간을 할당 받음 : kernal mem

kernal mem을 multiple slab으로 나눔

각각의 slab은 multiple pages로 구성됨

* 연속된 contiguous page

slab마다 다른 size의 object를 관리한다



Cache

* 하나나 그 이상의 slab으로 이루어짐

* 자주 사용되는 data
* 특정한 사이즈의 오브젝트만 사용할 수 있는 메모리 공간
* 컴퓨터 구조에서 배우는 캐시랑 다른 애임 (slab allocator에서 사용되는 캐시)
* 커널 데이터를 위해 미리 캐시만큼 할당해놓음
* object 생성 요청이 들어오면 해당 크기만큼의 캐시를 찾아 하나 매핑해줌
* 캐시는 처음부터 크게 잡지않음
* 캐시가 꽉차면 빈 slab하나를 더 가져와서 캐시 크기를 늘린다



장점

fragmentation이 없음

* slab단위로 캐시에 할당
* 캐시안을 실제 커널에서 사용하는 메모리 사이즈로 줄여둬서 internal fragmentation 안생김

* external은 있을수도있음

메모리 request를 할 때도 해당하는 size들이 캐시에서 할당받은 것이기 때문에 그 주소만 받아오면 바로 사용할 수 있다 <뭔소리?

따라서 fast memory request satisfaction이 가능하다



## I/O Devices

지금까지는 cpu, 메모리에서 os가 해줘야할거 배움

  Many I/O device도 있다...

피씨에 장착이 됨



Port : 디바이스와 프로세스를 연결하는 포인터

* 포트는 제한적
* 포트에 expansion bus interface를 연결해서 하나의 포트를 여러개로 나눌 수 있음. 나눠진 거에 추가적으로 컴포넌트 연결 가능 (느린 device일 경우 가능)

Bus : 포트들을 프로세스와 연결

* shared bus
* daisy chain(직렬 연결) : device to device

Controller

* 호스트 어댑터라고도 함
* I/O 디바이스를 컨트롤함
* I/O디바이스 자체에 장착되기도 하고 GPU같은 경우는 메인보드에 별도로 장착

* 또 다른 작은 컴퓨터, 간단한 프로세싱을 할 수 있는 프로세서와 메모리, 컨트롤러가 있음



### I/O Hardware

컨트롤러에는 레지스터들이 있다

* status reg : busy(1bit)
* control reg : 명령어가 들어감
* data -in/ -out : 시스템으로 들어오는/호스트가 디바이스한테 보내주는 데이터

status, control reg같은 경우는 적은 byte에 저장(1-4), data-in, out같은 경우는 여러 개의 데이터를 FIFO buffer를 이용해서 여러 개의 데이터를 동시에 보관

큰 동작

cpu가 status reg 값 확인(놀고있나 일하고있나)

놀고있으면 control reg한테 일해달라고 요청

cpu한테 전달할 data가 있으면 data-in reg에 값을 써둠

레지스터들을 통해서 디바이스와 호스트 프로세스간의 소통이 이루어짐

각각의 레지스터를 지칭할 주소가 필요

주소는 어떻게 관리?

Memmory-mapped I/O - 메모리를 따로 빼두고 씀

* 사용하는 레지스터 공간이 클 때 사용
* 일반적으로 port- mapped io 사용

Port-mapped I/O - 포트별로 관리





## Polling and Interrupt

### Polling

호스트가 일을 시키는데 아이오 디바이스가 일을 완료하는데 시간이 오래걸림

디바이스가 직접 알려주지않고 호스트가 매번 일이 됐는지 확인하는게 폴링



handshaking

* host cpu는 io device controller 한테 일을 시키기전에 busy bit 확인
  * 1이면 기다림 0이면 일을 시킬 수 있음 (지속적으로 확인) <- polling
  * 디바이스가 느리면 자주 확인해야함
  * 확인하는 정도를 잘 조절해야함

* controller reg한테 read인지 write요청인지 써둠 (write일 경우 어떤 data가 나가는지도 data-out reg에 함께 써둠, 주소도 같이 전달)
* 일을 시키고자 하는 정보들이 전부 전달이 됐으면 command-ready bit을 1로
* i/o컨트롤러는 커맨드 레디 비트가 1이되면 busy bit을 1로 만들어줌
* 커맨드에 해당하는 transfer를 수행
* 모든 data transfer이 끝나면 busy bit, error bit, command-ready bit 을 0으로



### Interrupts

> 폴링은 하는데 3개의 명령어만 필요
>
> * status reg로부터 값을 읽음
> * 읽은 bit중에 ready bit에 해당하는 부분만 &를 사용해서 뽑음
> * 1인지 0인지 판단

간단하지만 문제는 이걸 자주 해야함



그래서 interrupt 사용

interrupt는 디바이스 컨트롤러가 호스트한테 직접 알려줌

interrupt가 들어오면 cpu는 자기가 하던 instruction을 마치고 interrupt에 해당하는 interrupt handler를 불러서 수행

핸들러가 끝나면 원래 하던 일을 복구



만약에 디바이스 드라이버가 I/O를 초기화하는데 I/O의 크기가 매우 크면

ex) 10MB 짜리 파일을 메모리에 저장해야할 때

* 10MB = 4KB x 2560
* I/O가 2560번의 초기화를 해야함





### DMA (Direct Memory Access)

io가 cpu한테 4kb보내주면  cpu가 그걸 메모리에 쓰는게 아니라 중간에 DMA가 존재

DMA가 4KB 데이터를 메모리에 직접 쓰게 하자

10MB다 끝나면 DMA가 알려주자

* large data movement같은 경우엔 DMA가 대신 일을 해줘서 cpu의 부담을 줄임
* cpu를 bypass해서 바로 io에서 memory로 데이터를 써주는 효과를 내자
* cpu는 처음 초기화할 때만 관여
* DMA가 mem에 데이터 저장
* 일이 다 끝나면 DMA가 cpu한테 알려줌 dma가 인터럽트를 보냄



cpu 는 dma한테

작업의 크기

어떤 명령어인지

src, dest의 주소등을 알려줌



