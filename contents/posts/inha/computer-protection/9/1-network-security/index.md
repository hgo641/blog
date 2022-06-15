---
title: "9-1 "
date: 2022-04-30
tags:
  - computer protection
series: "컴퓨터 보안"
---



## 패킷(Packet)

네트워크에서 데이터를 주고받는 단위

* 네트워크 3계층에서 정의되는 데이터 단위
* 그 외에도 OSI의 각 계층에서 주고 받는 정보의 단위를 모두 패킷이라고 총칭하기도함
* 인터넷(Internet)에서는 IP 데이타그램을 패킷이라고 말함
* 헤더와 데이터로 구성됨
* 헤더 : 운송장, 데이터 : 물품



## Packet Sniffing

> src가 dst에게 보내는 패킷을 중간에 훔쳐봄

* 네트워크 트래픽을 도청하고 민감한 정보를 수집함
  * 노출된 ID, password, cookie등을 엿볼 수 있음
  * HTTP Cookie : 평문 형태의 정보 (session hijacking) (평문이 아니라 암호문이라면 패킷이 노출되어도 정보 노출 위험을 최소화시킬 수 있긴함)
* 다운로드 불가능한 파일을 덤프하거나 엿볼 수 있는 위험
* Passive attack



### 스니핑이 가능한 조건

* 스위치/라우터로 분리된 네트워크들 간에는 스니핑 불가
  * 허브로는 가능 (브로드캐스트)
* 묶여진 내부 네트워크에서만 스니핑 가능
  * 예시 사진 1번에서는 가상 네트워크 구역내에서만 스니핑 가능
  * 예시 사진 2번에서 A공유기 구역의 pc가 B공유기 구역에 스니핑 불가능



## Wireshark

패킷 분석 프로그램



### Promiscuous mode란?

> 네트워크 패킷 필터링을 하지않는다

정상적인 경우, 네트워크 패킷들을 하드웨어적으로 필터링함

* 네트워크 카드에 인식된 2계층(MAC)과 3계층(IP) 정보가 자신의 것과 일치하지않는 패킷은 무시한다
* 무시하지않고 모든 패킷을 일일이 CPU가 확인하여 처리하면 CPU에 부하가 많아짐

Promiscuous mode를 하면 패킷을 필터링하지않는다

ex) 와이파이 패킷을 promiscuous mode로 해두면 컴퓨터에서 패킷을 확인했는데 핸드폰의 패킷도 관찰 가능 



> Promiscuous 모드로 모든 패킷을 확인하므로 나에게 필요한 패킷을 필터링하는게 중요하다



### Capture filter

> 캡처를 시작하기전에 필터를 적용하는 방식
>
> 불필요한 패킷을 사전에 필터링해서 Wireshark 프로그램에 걸리는 부하를 줄일 수 있다

* 필터없이 캡처를 시작할 경우 많은 양의 패킷으로 인해 원하는 패킷을 찾기 힘듦
* 캡처를 시작한 이후 Display Filter를 적용할 수 있으나 Display Filter는 모든 패킷을 캡처한 후 필터링을 진행하는 것이기 때문에 불필요한 패킷이 캡처되어서 파일의 크기가 커진다

ex)

* host 10.10.50.170 : 특정 호스트만 캡처

* port 80 : 80번 포트만 캡처
* host 10.10.50.170 and port 80 : &&와 ==, || 연산자 사용 가능



### Display Filter

> 캡처를 한 뒤 원하는 패킷만 화면에 출력

* Capture filter와 문법이 다름

ex)

* ip.addr = 10.10.50.170
* host 10.10.50.170 and host 10.10.50.15
* tcp.port = 80



### 예제2 : FTP Packets

#### FTP (File Transfer Protocol)

* 파일 전송 프로토콜
* FTP는 통신을 위해 2개의 port를 사용함 (command, data transfer)
* 보통, command용으로 20번 포트, data-transfer 용도로 21번 포트를 사용한다
* 평문 파일 전송 프로토콜이라 해킹이 쉬움





## MAC Address

* NIC(네트워크 인터페이스 카드)에 할당된 고유한 식별자
* 로컬 네트워크 통신에 사용
* 48-bit의 주소길이
* 관리자 계정은 PC의 MAC주소를 직접 변경할 수도 있음
  * LAN카드내에 박혀있는 하드웨이 맥주소를 바뀔 수 없지만 PC에 로드되어있는 맥주소는 변경이 가능하다

> cmd에 ifconfig 명령어로 맥주소 확인 가능(리눅스, 맥)
>
> 윈도우는 ipconfig/all 



## LAN과 WAN (Network layer)

* Datalink layer 통신을 위해서는 MAC  주소를 알아야함

* 로컬 네트워크 (LAN) : 서버의 NIC와 내 컴퓨터의 NIC끼리 통신
* 외부 네트워크와 연결 (WAN) : 다른 LAN과 연결됨 (라우팅)  ex. 인터넷



## ARP 메커니즘

> ARP(Address Resolution Protocol) 
>
> * 맥주소를 알기위한 프로토콜



#### ARP를 활용하여 목적지의 MAC주소 찾는 방법

* Broadcast Request 
  * ip 주소를 가지고 브로드캐스트함
  * 이 ip주소 가지고 있는애 누구야!
* Unicat Reply (1대1)
  * 해당 ip주소를 가지고 있는 애가 자신의 mac주소를 답변해줌

#### ARP 취약점

답변을 해준 시스템이 정상적인 노드인지 보장 못함 (인증없음)

원하는 맥주소를 가지고있는 애가 아님에도 속이고 reply 보낼수도있음 - ARP Request



#### MAC주소를 이미 알고있음에도 ARP요청을 보내는 경우

* 브로드 캐스트가 아니라 유니캐스트로 보냄
* MAC주소가 바뀔수도있기때문
* 똑같은 ip를 쓰지만 컴퓨터가 바뀔수도있음
* 30초정도마다 ARP요청을 다시 보냄
* 보냈는데 응답이 없으면 다시 브로드캐스트



### ARP cache

> ARP cache table
>
> * MAC 주소와 IP주소를 매핑하고 있는 테이블

1. static ARP cahce entry
   * 수동으로 추가 가능 (컴퓨터 재시작시 삭제)
2. dynamic ARP cache entry
   * ARP reply packet을 받으면 OS에 의해 자동으로 저장
   * 30초간 지속



## ARP Spoofing

spoofing : 위장

> 근거리 통신망(LAN)하에서 ARP 메시지를 이용하여 상대방의 데이터 패킷을 중간에서 가로채는 중간자 공격 

* 가로챈 패킷을 변조하는 Active attack도 가능
* 데이터 링크(Layer 2)상의 프로토콜인 ARP프로토콜을 이용하기 때문에 근거리 상의 통신에서만 사용할 수 있는 공격



### 스위치로 묶인 네트워크에서 스니핑 시도

> 허브는 패킷을 브로드 캐스트로 모두에게 보내기때문에 Promiscuous 모드를 키면 패킷을 받아볼 수 있지만 스위치는 MAC주소 테이블을 사용해서 해당 pc에게만 패킷을 보내기때문에 Promiscuous mode를 해도 패킷을 훔쳐볼 수 없다.

#### 스니핑을 위해 스푸핑 시도!

> 스니핑 : 도청
>
> 스푸핑 : 패킷이 나를 거치게 함

1. 공격 대상에게 조작된 정보를 보내 수신지 변경 - 스위치의 맥주소테이블을 감염시킴
2. 공격자의 NIC는 promiscuous 모드로
3. 수신된 패킷은 relay함 (Packet forwarding) : 잘못된걸 눈치채지못하게 원래 받아야 할 애한테 다시 보내줌
4. 패킷 모니터링 툴 실행



## ARP spoofing 탐지 방법

결국은 스위치가 잘해줘야함

* ARP 탐지 솔루션 또는 네트워크 방화벽
  - arpwatch(\*nix 계열)
  - xarp(NT/\*nix)
  - ai 기반 탐지?
* Promiscuous 모드로 동작하는 host탐지
* ARP cache 테이블을 정적으로 유지



## DNS spoofing

> DNS Server가 올바른 Reply를 해주기전에 공격자가 Reply를 해줘서 웹을 연결시킴
>
> 이후 들어오는 DNS Server의 Reply는 무시됨