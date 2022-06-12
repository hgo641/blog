---
title: "os 13-2"
date: 2022-06-11
tags:
  - OS
---

## Hierarchical Page Tables

페이지 테이블이 여러개 (ptbr이 여러개)

* outer page table : main page table의 시작점을 관리
* (main) page table

p1 : outer page table에서 어떤 인덱스를 검색할지

* 1024개의 entry 
* 각 엔트리가 4B일 때 전체 4KB
* 온전히 하나의 프레임에 들어갈 수 있음

p2 : 실제 frame이 있는 위치

* 1024개의 entry

d : 오프셋. 실제 frame이 있는 위치에서 오프셋만큼 건너 뛴 곳에 찾고자하는 리소스가 있음



* PTBR -> outer page table의 시작주소를 가리킴 

> PTBR -> outer page table -> page table -> physical memory



> 계층화 구조라고 TLB까지 다단계가 되진않음



## Intel 32 and 64-bit Architectures

실제 사용 예시를 봐보자!

### IA-32

인텔의 32bit 구조

* 세그멘테이션 한 번 하고 페이징 한 번 하고
* 



`segmentation unit`이 또 다른 logical address인 linear address를 만듦



* 하나의 프로세스당 최대 16K개의 세그먼트로 나눌 수 있다.
  * 16K = 2^10 x 2^4 = 14bit

* 각각의 세그먼트는 최대 4GB이다.
  * 2^2 x (2^10)^3 = 32bit

세그먼트를 크게 두 분류로 나눔

* LDT (local descriptor table) - 8K
  * private process
  * 다른 프로세스와 공유하지않는 공간
* GDT (global descriptor table) - 8K
  * 공유 가능



![](./ia-32.png)

logical address = (selector, offset)

selector는 16bit이다 (segment를 표현)

* 14bit + protection bit(2)

offset 은 32bit. 

로지컬 어드레스는 총 48bit



* 셀렉터를 보고 descriptor테이블을 찾음
  * g는 GDT인지 LDT인지
  * descriptor table에서 s만큼 떨어진 segment descriptor를 찾음 
  * 찾은 segment와 offset을 더해서 32bit linear address를 만듦

* linear address를 가지고 페이징을 한다
  * Hierarchical paging과 동일
* Page directory
  * page table들의 시작주소와 4MB 페이지의 시작 주소를 모아둠
  * 인텔 32에서는 4KB뿐만아니라 4MB 페이지도 지원
  * 모두 작은 사이즈로 하면 페이지 테이블의 사이즈가 매우 커짐
  * 큰 페이지에 저장해야할 경우도 있어서 4MB도 지원...
  * 4MB에서는 
    * P1(page num), d (page offset)



64bit은?

계층이 더 나눠짐

P1, P2, P3, P4, P5 ...



## Virtual Memory

로지컬메모리를 virtual memory라고 부름

virtual memory가 왜 생겨났나?

> 어떤 프로그램이 실행되면 프로그램에 필요한 코드가 전부 메모리에 올라옴
>
> 비효율적
>
> 전부 올라와야할 필요가 있을까...?
>
> Error code, unusual routines
>
> * Partially-loaded 하자!
>   * 피지컬 메모리 limit가 줄어든다
>   * 더 많은 프로세스가 동시에 메모리를 사용가능
>   * degree of multiprogramming이 올라감
>   * response time증가하지않음



 프로그램의 부분만 메모리에 올리자

* logical addr 이 physical addr보다 커져도 됨



### Virtual address space

> 기존 방법은 처음에 다 올려놓고 필요없는걸 뺐다면 이번에는 필요할 때 올린다.  요구할 때 페이징 수행

차이 : 

한 번도 요구된적이 없는거라 physical memory에 없을수도있음

그럴땐 HDD에 접근해서 가져와야함

* virtual address space는 0번부터 max까지 있음
  * 상위에는 stack 하위에는 code



## Demand Paging

virtual memory의 핵심적인 차이점!

페이지를 필요할 때 가져다놓자!

> 불필요한 I/O가 줄어든다
>
> 메인 메모리를 더 적게 차지

스와핑을 하는 페이징 시스템과 유사



