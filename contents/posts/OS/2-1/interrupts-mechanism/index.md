---
title: "Interrupt 메카니즘"
date: 2022-03-30
tags:
  - OS
series: "OS"
---

# Interrupt

CPU 외부에서 CPU한테 이벤트를 알려주면서 CPU를 멈추게함
각각의 인터럽트는 cpu가 수행할 인터럽트 서비스 루틴(ISR)이 존재

Interrupt vector : inturrupt와 ISR을 매칭시켜놓음. 모든 서비스 루틴에 대한 주소를 가지고 있음.
