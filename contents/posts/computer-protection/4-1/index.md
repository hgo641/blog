---
title: "다양한 보안 위협"
date: 2022-04-14
tags:
  - computer protection
series: "컴퓨터 보안"
---

## Malware (malicious software)

허가되지않은 일을 하기 위해 악의적으로 설계된 소프트웨어

(의도적이지 않은 결함은 bug라고 하며 멜웨어는 의도적인 소프트웨어)

- 트로이 목마 : 정상 프로그램으로 위장한 악성 프로그램
- Root kit : 시스템의 root 권한을 획득하고 악성코드의 존재를 숨김
- Backddor : 정상 인증 절차를 우회하여 시스템에 접속할 수 있게 하는 뒷문
- 감염형 malware
  - Virus : 정상처럼 보이는 프로그램 안에 숨음, 다른 프로그램들 감염
  - Worm : 얘 자체가 프로그램, 능동적으로 전파, 스스로 복제
- 랜섬웨어

### Ransomware (ransom + ware)

피해자의 기기 및 데이터를 장악(주로 암호화)해서 접근이 불가하게 만들고 민감한 데이터를 공개하는 것을 막거나 데이터를 복원하려면 비용을 지불하도록 유됴

---

## 블랙마켓

암시장

온라인으로도 유용됨

제품 : 신용카드, 체크카드, 페이팔 계정, 개인정보, 악성프로그램등

서비스 : 호스팅, DDoS 봇넷, 소프트웨어 불법 대여, ranswomware as a service (RaaS) 등

ex) 실크로드

### 실크로드

최초의 현대적인 darknet marker

`다크웹`을 이용하기 때문에 일반 인터넷으로는 접근 불가능

지금은 없음

---

## Darknet

접근을 위해서는 특정 소프트웨어가 필요한 오버레이 네트워크

주로 소규모 P2P 네트워크로 연결되거나 Tor, Freenet, ITP 등 별도적인 소프트웨어 상에서 동작

### Tor (The onion router)

대표적인 다크넷 제공 오픈 소스 소프트웨어 중 하나

`onion routing`을 사용한 traffic anonymization technique 적용

사용자의 네트워크 사용 패턴 및 위치를 숨김으로서, Network surveilance 나 traffic analysis 방지

`onion routing` : 라우팅이 숨겨짐. 트래픽 익명화. 쓰는 사용자가 많아야 유리

악용 사례만 있는 것은 아님. 기밀성 보장

## Deep Web

일반 검색 엔진으로는 드러나지 않는 웹 컨텐츠

얘도 악용 사례만 있는 것은 아님. 일부가 다크웹으로 사용됨

넓은 의미에서는 웹 메일(ex. gmail)의 inbox, OTT(ex. netflix)의 컨텐츠 등을 포함 : gmail 주소로 바로 gmail에 들어갈 수 없음

대부분 합법적인 사이트

---

## 0-day

0-day 공격은 컴퓨터 소프트웨어의 취약점을 공격하는 기술적 위협으로 해당 취약점에 대한 패치가 나오기 전에 이루어진다.

공격의 수행 사용자와 프로그램 개발자가 잘 모르는 것이 보통

공격에 사용되는 코드를 exploit이라고 함

## APT (advanced persistent threat)

0-day들을 조합해서 사용하는 고급 공격

다양한 보안 위협들을 양산하여 특정 대상에게 지속적으로 가하는 일련의 행위

ex) shady rat, stuxnet(산업 시설 감시 및 파괴)

## Steganography

미디어 파일안에 데이터를 숨기는 기술
