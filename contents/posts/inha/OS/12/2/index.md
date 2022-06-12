---
title: "os 12-2"
date: 2022-06-11
tags:
  - OS
---

## Contiguous Allocation

### Compaction

hole들을 모음

external fragmentation 해결

문제 : I/O를 발생시킴

큰 메모리 카피를 일으킴 - 오버헤드

지금은 쓰이지않음

### Swapping

fragmentation을 해결한건 아니고 여유공간 자체를 늘려줌

Backing store : 스왑되는 공간. 기존에 있는 프로세스 메모리 공간 하나를 디스크로 백업해놓음

- 백업을 시키고 free.
- free한 공간에 새 프로세스 할당

근데 오버헤드는 compaction과 비슷;

- 데이터를 빼오고 이동시키는 시간이 길다
- 그러나 여유공간이 많은 것 처럼 보여줌 Degree of Multi programming을 늘림
- but, disk에 있는 애들을 위한 ready queue를 추가로 관리해줘야함

### Context Switch Time Including Swapping

컨텍스트 스위칭을 할 때 Swapping을 같이하자!

- 디스크에 옮겨놓는 일은 시간이 오래걸림
- Context switch time이 커질 수 있다
- Standard swapping은 쓰이지 않음(이후 Paging과 함께 쓰이긴 함)
- 모바일 os에서는 swap이 사용되지않음
  - 메모리에 있는 data를 디스크로 옮김
  - 모바일은 디스크가 없음
  - Flash memory based임
  - 플래시 메모리는 write cycles가 제한되어있음 ( 같은 공간에 쓰기 횟수가 제한)
  - 모바일 os에서는 refresh : 백그라운드에 있는 애들의 정보를 계속 유지할 수 없음. 최소한의 정보만 저장해두고 free시킴

## Contiguous Allocation의 문제

모든 데이터를 통째로 관리

코드는 여러 프로세스가 공유될 수 있음

한 번 만들어진 코드는 변하지않음. read-only

그러나 데이터, stack, heap섹션은 일반적으로 공유되지않음- read-write

즉, 특성이 다른 영역들이 존재

> 프로세스의 모든 공간(stack, heap, data, code)를 전부 이어붙여 저장하지말고 따로따로 저장할 순 없을까?
>
> -> Segmentation

## Segmentation

메모리 area를 여러 개의 area로 나누자

- segment마다 개별적으로 base와 limit 값을 가지고있어야함
  - 두 개의 protection bit가 필요(read write가 가능한지 표시하는)
  - ex) 10이면 read만 가능

MMU가 할 일

1. 지금 요청이 온게 어느 seg에 속하는 것인가
2. 해당 seg의 limit을 검사
3. 해당 seg의 base를 더함

어느 seg에 속하는지 어떻게 알 수 있을까?

- logical address의 상위 bit을 세그먼트를 구분하는데 쓰자
- 인스트럭션 자체에서 구분을 해보자
  - ex) stack에 푸쉬하는 인스트럭션이니 stack에 가야겠구나!
  - 인스트럭션마다 명확하게 나뉘는게 아니라 어려움

세그먼트별로 리밋과 베이스를 어떻게 관리할까?

segment table

## Segmentation Architecture

- 로지컬address에서 바라보는 주소를 다음과 같이 만듦

  - <segment-number, offset>
  - 상위 bit : 어떤 seg에 속하는지
  - 하위 bit : seg안에서의 주소

- Segment table

  - 각각의 seg에 할당된 base와 limit을 관리
  - s : seg의 번호
  - d : = offset, seg안에서의 addr

![](../../../../../../inha-image/segtable.png)

base : physical addr의 starting을 담고있음

limit : 해당 seg의 length

레지스터 두 개가 필요

- Segment-table base register(STBR)
  - physical memory안의 세그먼트 테이블의 실제 위치
  - seg-table의 base
- Segment-table length register(STLR)
  - 해당 seg table의 length를 구하는 테이블
  - seg-table의 limit

Process의 수 x segment의 수만큼 레지스터가 필요함

- hign overhead
- 레지스터로 관리하긴 힘들고 메모리 안에 위치시킴
- 어떤 addr에 request가 들어오면 그 addr이 속하는 seg table정보를 읽어옴
- seg table의 정보를 읽어보면 그걸 바탕으로 limit검사하고 base를 더함
- 실제 physical addr로 변환
- 세그먼트 테이블의 정보도 메모리에 있어서 읽어와야함. 메모리에 STBR, STLR 저장 (세그먼트 테이블의 base와 limit) : 한 번 요청하면 두 번 읽어와야함
- 메모리 access 횟수가 두 배가 됨

각 seg마다 base와 limit이 다름.

여러 seg로 나눠도 여전히 큼

큰 애들을 메모리에 할당

그런 상태에서 할당과 해제를 반복하면 hole이 불균형하게 생기고 못쓰는 hole이 많이 생김

여전히 external fragmentation 많이 생김

그래서 나온게 Paging

## Paging

Modern OS

> segmentation은 여전히 external fragmentation이 심함

메모리 fragmentation을 줄이자

스와핑을 좀 더 효율적으로

<br/>

메모리 공간을 균등한 사이즈로 나눔

- fragmentation이 발생하는 이유는 프로세스가 사용하는 메모리 공간이 서로 불균형하니까 작은 애들이 free, allocation을 반복하다보면 큰 공간이 점점 작게 나눠지고 홀이 불균등하게 생김
- 큰 요청이 들어왔을때 들어갈 공간이 없음
- 페이징은 모든 공간을 고정된 사이즈의 frame으로 나눔
- 프레임의 크기는 2^n으로 나눔 (os에서는 기본적으로 4KB)
- 4KB는 하드디스크의 블록 사이즈. 한 번에 단위별로 가져오게 하려고 맞춤

<br/>

- 로지컬 addr을 똑같은 사이즈로 자름 ( page라고 부름 )
- segmentation하듯이 할당함. page별로 frame에 매핑해줌.

  - ex) 15KB짜리 프로그램을 4KB인 4개의 프레임에 할당

- 사용 가능한 free frame들을 전부 체크해야함
- page -> frame의 정보를 관리하는 정보가 커짐...
- 여전히 internal fragmentation존재
  - 15KB프로그램에 16KB할당
