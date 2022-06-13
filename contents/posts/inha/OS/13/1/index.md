---
title: "os 13-1"
date: 2022-06-11
tags:
  - OS
---

## Address Translation

logical -> physical로 바꾸는 것

<p, d>

p : page number (20bit) = 최대 2.5MB

d : page offset (12bit) = 2^12 = 4KB

총 32bit

> internal fragmentation 이 발생하지만 크진않다

 <br/>

프로세스마다 페이지테이블 1개

프로세스마다 필요한 페이지 개수가 다르기때문에 페이지 테이블 크기도 다름

<br/>

페이지 테이블들은 메인 메모리안에 있다.

PCB에는 PTBR, PTLR이 있음

* PTBR : 메인 메모리 안에 있는 프로세스의 페이지 테이블 시작점을 가리킴

* PTLR : 페이지 테이블의 길이



## TLB

자주 사용하는 <페이지 넘버:프레임 넘버>를 저장해서 MMU에 바로 전달

TLB를 통해서 메인 메모리의 페이지 테이블에 갈 필요없이 메모리 access가능 



TLB에는 이전 프로세스가 사용하던 p:f가 저장되어있음

* 이전 프로세스의 것은 다 지원주든지
* valid bit을 추가할 것인지 : 0이면 무의미한 번호

valid bit(ASIDs) : 유의미한 TLB정보인가

1. load/store instruction 
2. tlb search
   1. tlb hit => mmu translation => physical memory access
   2. tlb miss => mmu에서 ptbr을 가지고 page table에서 원하는 page number에 대한 frame번호를 읽어옴 => translation => physical mem access



페이지 테이블은 페이지 넘버에 대한 프레임 넘버를 기본적으로 가지고 있다

* 추가적으로 read-only등을 나타내는 protection bit을 가지고 있을수도 있다 (segmentation과 함께 사용 가능. 세그먼트별로 서로 다른 기능 제공)



페이지의 길이를 PTLR로 표시할 수도 있지만 어떤 시스템에서는 무조건 0~2^20-1까지 전부 가져갈 수 있게한다.

* 페이지 테이블내에 사용하지않는 페이지가 있을수도있음 - valid bit으로 사용여부를 알려줌

* 실질적으로 쓰이지않는 6, 7번 페이지



## Shared Pages

페이지 테이블에 있는 수는 실제 frame번호들

페이지 테이블을 같은걸 지정해놓으면 페이지 공유 가능.

물리적으로 같은 주소를 가리킴

32bit주소 

주소는 0 ~ 2^32-1까지의 수로 표현 가능

page(p) 는 0 ~ 2^20-1개 생김

페이지 테이블은 frame번호를 기록

* frame(f) 번호 : 20bit

page table은 250MB인데 > 하나의 페이지가 4KB

* 페이지 테이블이 여러 개의 페이지에 저장된다는걸 의미
* PTBR하나로만 페이지 테이블을 가리키기 쉽지않음
* hierarchical page table을 통해 해결



