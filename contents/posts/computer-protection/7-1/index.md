---
title: "7-1 DoS attack"
date: 2022-04-20
tags:
  - computer protection
series: "컴퓨터 보안"
---

## DoS attack : 서비스 거부 공격

- 취약점 공격형
  - Boink, Bonk, TearDrop
  - Land
- 자원 고갈형
  - Ping of death
  - SYN flooding
  - HTTP flooding
  - Smurf
  - Mail bomb

### TearDrop

- TCP 오류 제어 로직의 불완전한 구현을 악용
- 클라이언트 측에서 의도적으로 패킷의 시퀀스 넘버와 길이를 조작
- 패킷들이 부분적으로 겹치거나 빠진 패킷이 있어 reassemble 불가

> 대응
>
> - 수신되는 패킷의 frame alignment를 확인하여 filtering

### LAND (local area network denial) Attack

- 일종의 packet spoofing 공격
- source 와 destination IP address 를 모두 target host 의 IP address로 설정하고 TCP SYN 패킷을 전송
- target host는 자신에게 지속적으로 응답을 하게됨( 보내는 사람도 나... 받는 사람도 나...?)

> 대응
>
> - source와 destination IP address 유효성 검증

---

### Ping of Death

- ping 명령어에 최대한 긴 패킷을 실어서 공격 대상에게 전송
- IPv4 패킷 크기는 65535 바이트까지 가능하나 경유 네트워크 특서엥 따라 전송중에 수백 - 수천개의 패킷으로 분할됨
- 분할된 패킷이 다시 합쳐져서 전송되는 일은 거의 없으므로 공격 대상 시스템은 대량의 작은 패킷을 수신하게 됨

> 대응
>
> - 주로 방화벽에서 ping이 사용하는 ICMP를 차단하여 해결

### SYN flooding

- 시스템에서 허용하는 동시 사용자 수 제한과 TCP 3-way handshake 특성을 악용
- 공격자는 가상의 클라이언트로 위조한 SYN 패킷을 여러 개 만들어 서버에 보냄으로써 서버의 가용 동시 접속자 수를 모두 점유 ( 모두 SYN Received 상태로 만듦)

> 대응
>
> - SYN Received 상태의 대기 시간을 줄이는 등 다양한 해법 존재

### HTTP flooding

웹 서버에 대한 도스/디도스 공격

**기본적인 공격**

- Get flood
  - image와 같은 특정 static content를 지속적으로 요구
- POST flood
  - 서버 내의 database 검색 등 특정 동작을 지속적으로 요구

> 대응
>
> - traffic profiling(특정적인 HTTP 요청 패턴을 검출하고 차단)

**보다 진화된 공격**

- slow HTTP POST (= RUDY (RU-Dead-Yet?))
  - 서버가 POST 데이터를 모두 수신하지 않았다고 판단하면 전송이 다 이루어질때 까지 연결을 유지하는 특성을 활용, POST 메소드로 대량의 데이터를 장시간에 걸쳐 분할 전송하여 연결을 장시간 유지함으로써 서버의 자원 잠식
  - 예를 들어 Content-Length를 100000byte로 하고 데이터는 일정한 간격으로 1byte씩 전송
- slow HTTP header (slowloris attack)
  - HTTP header 정보를 비정상적으로 조작하여 웹서버가 완전한 header 정보를 기다리도록 함
  - 예를 들어, HTTP에서 header의 끝을 개행문자 /r/n (CR LF)로 구분하므로, header 시작 후 개 행 문자 없이 의미없는 문자열을 보내면 서버는 계속 연결을 유지

> 대응
>
> - 세션 임계치 설정, 세션 타임아웃 시간 제한

### Smurf attack

- Direct broadcast
  - 기본적인 broadcast는 destination IP address 255.255.255.255로 패킷을 전송하며, 이는 라우터 경계내에서만 동작함
  - direct broadcast: 라우터를 넘어가서 broadcast를 해야 하는 경우 IP address 의 일부분만 broadcast 주소로 채움
- Smurf attack
  - Direct broadcast를 악용하여, source IP address를 특정 주소(피해자 의 IP address)로 조작한 ICMP request 를 broadcast
  - 피해자는 수많은 ICMP reply를 받아 과부하 상태가 됨

> 대응
>
> - 각 호스트와 라우터들로 하여금 broadcast address 일 경우 무시하도록 설정
