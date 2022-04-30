---
title: "9-1"
date: 2022-04-30
tags:
  - computer protection
series: "컴퓨터 보안"
---





## 패킷(Packet)

* 네트워크 계층 (Layer 3)에서 정의되는 데이터 단위
* 그 외에도 OSI의 각 계층에서 주고 받는 정보의 단위를 모두 패킷이라고 총칭하기도함
* 인터넷(Internet)에서는 IP 데이타그램을 패킷이라고 말함
* 헤더와 데이터로 구성됨
* 헤더 : 운송장, 데이터 : 물품



## Packet Sniffing

* 네트워크 트래픽을 도청하고 민감한 정보를 수집하는 행위

* passive attack

* 노출된 ID, password, cookie등을 엿볼 수 있는 위험

  > HTTP Cookie
  >
  > * 웹 사이트에 접속할 때 웹 사이트 서버가 내 컴퓨터에 보내는 임시 파일이다
  > * 크기는 4KB 이하로 매우 작다

* 다운로드 불가능한 파일을 덤프하거나 엿볼 수 있는 위험

* 스위치/라우터로 분리된 네트워크들 간에는 sniffing 불가

  * A공유기 구역에서 B공유기 구역으로 sniffing 불가능
  * 허브는 가능. broad cast 가능

  

## Wireshark

패킷 분석 프로그램

### Promiscuous mode란?

> 정상적인 네트워크 카드는 네트워크 카드에 인식된 2계층(MAC)과 3계층(IP) 정보가 자신의 것과 일치하지 않는 패킷은 무시한다
>
> 정상적인 경우에는 이러한 패킷들을 처리할 이유가 없기 때문
>
> 모든 패킷을 일일이 CPU가 확인하여 처리하면 CPU에 부하가 많아짐

* 위의 필터링을 끈다 
* 2계층과 3계층에서 필터링을 하지 않는다

### Capture filter

* 그렇다고 다 필터안하면 너무 많음. 적당히 필터해야함
* 캡처를 시작하기 전에 필터를 적용하는 방식
* 필요성1 : 필터없이 캡처를 시작할 경우 많은 양의 패킷으로 인해 원하는 패킷을 찾기 힘들다
* 필요성2 :  캡처를 시작한 이후 Display Filter를 적용할 수 있으나 Display Filter는 모든 패킷을 캡처한 후 필터링을 진행하는 것이기 때문에 불필요한 패킷이 캡처되어서 파일의 크기가 커진다.
* 이때 Capture Filter를 적용하면 불필요한 패킷은 사전에 필터링되어서 Wireshark 프로그램에 걸리는 부하를 효과적으로 줄일 수 있다

### Display Filter

* 캡처를 한 뒤 원하는 패킷만 화면에 출력한다
* Capture filter와 문법이 다르다
* 원하는 패킷을 캡처한 뒤 세부 분석을 하기 위해 쓰인다



#### 캡처 방법

* 특정 호스트의 패킷 캡처
  * ip.addr == 10.10.50.15
* 특정 두 호스트의 통신 패킷 캡처
  * host 10.10.50.170 and host 10.10.50.15
* 특정 포트 패킷 캡처
  * tcp.port == 80
* 특정 두 포트 전부 패킷 캡처
  * tcp.port ==80 or tcp.port ==1770
* 특정 호스트의 특정 포트 패킷 캡처
  * ip.addr == 10.10.50.170 and tcp.port == 80
* 특정 패킷만 캡쳐 안함
  * not arp
* 출발지나 목적지 IP 주소로 검색
  * ip.addr == 192.168.0.1
* 출발지 IP 주소로 검색
  * ip.src == 192.168.0.1
* 도착지 IP 주소로 검색
  * ip.dst == 192.168.0.1
* 포트로 검색
  * tcp.port == 443
* 출발지 포트로 검색
  * tcp.srcport == 443
* 도착지 포트로 검색
  * tcp.dstport == 443



### Practice 2 : FTP Packets

* FTP

  > * 파일 전송 프로토콜
  > * TCP/IP 프로토콜을 가지고 서버와 클라이언트 사이의 파일 전송을 하기 위한 프로토콜
  > * FTP는 통신을 위해 2개의 port를 사용함 (command, data transfer)
  > * 보통 FTP 프로토콜은 20번 포트 (for command)
  > * FTP-DATA 프로토콜은 21번 포트 (for data transfer)

* FTP를 사용하지 말아야 하는 이유 ?
  * **평문** 파일 전송 프로토콜이기때문



## MAC Address / IP Address

* MAC Address : Datalink 계층의 주소
* IP Address : Network 계층의 주소
* 도메인 이름은 정류장 이름
* IP는 정류장 번호
* MAC은 정류장 주소

### MAC(Media Access Control) 주소

* NIC(네트워크 인터페이스 카드)에 할당된 고유한 식별자
* 로컬 네트워크 통신에 사용
* 48bit의 주소길이
* 관리자 계정은 PC의 MAC주소를 직접 변경할수도 있음
  * window는 ipconfig/all
  * mac은 ifconfig

#### MAC Address 를 사용하는 이유

* 서버의 NIC와 내 컴퓨터의 NIC끼리 통신
* Datalink layer 통신을 위해서는 MAC 주소 알아야함



### ARP의 메커니즘

IP주소에 해당하는 MAC주소를 알고싶음 

* IP를 가지고 broad cast request로 IP에 해당하는 컴퓨터를 찾는다

* 해당 IP를 가지는 컴퓨터가 Unicast Reply로 자신의 MAC주소를 알려준다

  > ARP 취약점
  >
  > System B(컴퓨터)가 정상적인 노드인지 보장 못함(인증없음)
  >
  > B가 정말로 해당 IP를 가진 컴퓨터인지, 악의적인 목적을 지닌 공격자인지 판별할 수 없다 



ex) 

#### ARP Request : 1292.168.0.16?

* 192.168.0.50(00:E0:91:DB:3C)이 192.168.0.16의 MAC주소를 알기 위해 ARP Request 메시지를 브로드 캐스팅



#### MAC 주소를 알고있음에도 ARP 요청이 발생하는 경우

ex. 컴퓨터가 바뀐 경우? IP주소는 그대로 지만 MAC 주소 변경됨

이런 경우 주기적으로(30초마다) ARP요청. (유니캐스트)

reply가 오면 살아있고 응답이 없으면 문제가 생긴것. 알고있는 MAC주소가 유효하지않으므로 다시 브로드캐스트



### ARP cache

#### ARP cache table

* MAC주소와 IP주소를 매핑하고 있는 테이블

  1. static ARP cache entry
     * 수동으로 추가 가능. (컴퓨터 재시작시 삭제)

  2. dynamic ARP cache entry
     * ARP reply packet을 받으면 OS에 의해 자동으로 저장
     * 30초간 지속

 

## ARP Spoofing(위장)

* 근거리 통신망(LAN)하에서 ARP 메시지를 이용하여 상대방의 데이터 패킷을 중간에 가로채는 중간자 공격(Man-in-the-middle-Attack)기법이다.
* 가로챈 패킷을 변조하는 Active attack까지 수행 가능
* 이 공격은 데이터 링크(Layer 2)상의 프로토콜인 APR 프로토콜을 이용하기 때문에 근거리 상의 통신에서만 사용할 수 있는 공격이다



#### 스위치를 통하는 패킷의 흐름

* 유니캐스트 패킷
* 스위치가 MAC 주소 테이블을 유지/관리

* 스위치로 묶였다면 하드웨어적으로 연결을 해줘서 Promiscuous mode 효과 없음

* 허브로 묶였다면 모든 시스템을 다 봐서 Promiscuous mode 효과 있음
* 스니핑(도청) != 스푸핑

#### 스위치로 묶인 네트워크일 경우 어떻게 스니핑할까?

공격 시나리오

* 공격 대상(1번)에게 조작된 정보를 보내 수신지를 변경 (ARP 패킷 관련)
* 공격자의 NIC는 promiscuous 모드
* 수신된 패킷은 relay함 (Packet forwarding) : 정상인척 패킷을 받아서 보내줌
* 패킷 모니터링 룰 실행



## Detect ARP Spoofing

결국은 스위치가 잘해줘야함. (ex. 방화벽)

* ARP 탐지 솔루션 또는 네트워크 방화벽
  * arpwatch(\*nix 계열)
  * xarp(NT/\*nix)
  * ai 기반 탐지?
* Promiscuous 모드로 동작하는 Host 탐지
  * ARP Spoofing 공격 시도 가능성이 높기 때문
* ARP Cache 테이블을 정적으로 유지



## DNS(Domain Name System) Spoofing
