---
title: "5-1"
date: 2022-04-15
tags:
  - computer protection
series: "컴퓨터 보안"
---

CVE : 많이 나오는 공격들. 알려져있는 취약점

CWE : CVE에 기반해서 만든 분류 체계

CAPEC : 공격들을 추상화해서 신경써서 분석해야할 것들을 분류

OWASP (Open Web Application Securty Project)

웹 응용 보안을 다루는 오픈 커뮤니티

Broken Access Control : 말그대로 access control에 문제가 생김

Crypographio Failures : 암호화 하는 것에 문제

Injection : Cross-site Scripting이나 sql injection등 정상적이지않은 명령을 주입

Insecure Design : 보통 구현이 잘못되는데 얘는 디자인 자체가 잘못된

Security Misconfiguration : 보안 설정 잘못함

Vulnerable and Outdated Components : 취약점에 대한 패치가 나왔으나 적용을 안해서 여전히 취약점이 남아있음

Identification, and Authentication Failures 사용자 식별, 인증 실패

Software and Data Integriry Failures 인터그리티가 확인이 안됨. 전자서명이 안된 프로그램을 설치하거나

Security Logging and Monitoring Failures 사고가 발생되면 수습을 해야함. 경고를 주거나 분석, 포렌식을 하는데 문제

Server-Side Request Forgery 서버 취약점을 공격

## Web 기본 지식

보안 관점에서 브라우저는 프로그램이고 사용자는 프로그램이 아님

브라우저 : 사용자 입력을 받아 소통

사용자가 실제로 볼 수 있게 처리 : rendering

### 브라우저와 웹 서버 간의 통식

보통 http사용

보안성을 추가한 https

HTTP : 브라우저와 웹 서버 사이의 통신을 정의하는데 사용 TCP 프로토콜 상에서 돌아감

HTTPS : 보안 계층에 SSL/TLS도 추가. 웹 서버가 브라우저에게 인증서를 제공함으로써 보안성 있는 통신을 할 수 있도록 함

https를 사용하면 웹 통신이 안전할까?

어느부분은 맞고 어느 부분은 틀리다

브라우저와 웹 서버간의 통신은 안전할지라도 사용자라는 취약점이 존재해서 무조건 안전한 것은 아님

### 웹서버

웹서버 : 브라우저 요청 사항을 처리하는 일에 중점을 둔다.

get, post를 처리해줄 수 있는 최소한의 서버

ex) 아파치

파일, DB 등을 연동시키려면 서버 측 스크립트(Web Application Server, WAS)가 필요하다

코드를 실행시켜서 응답을 바로 생성하거나 중요한 정보를 얻기 위해 데이터베이스와 연동하는등의 역할

ex) Python, java

보통 웹서버는 잘 나와있는 솔루션을 뜨고 웹 어플리케이션 서버는 개발자가 개발

백엔드 서비스 : 서버 측 스크립트와 연결된 부가적인 서비스를 백엔드라고함

웹 서버의 백그라운드에서 실행되는 서비스

정보를 저장하는데 쓰이는 데이터베이스등이 있다

## Web Hacking Overview

Browser WS WAS

​ 1.err 조건. vaildatior

​ 부적절한데이터

​ 를 강제로 주입

​ 2. 여기까지오고 나중에 DB 가서 보니까 err

### Injecting Malicious Data

####

쿠키 : 세션 아이디 등 임시 정보가 담겨있음
