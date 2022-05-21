---
title: "멋쟁이사자처럼 X 넥슨 MOD Suppoters Hackathon 2주차 회고"
date: 2022-05-21
update: 2022-05-21
tags:
  - mod
series: "mod"
---

멋쟁이 사자처럼 X 넥슨 MOD의 2주차 교육 내용을 정리하는 포스트입니다.



## 자주 쓰이는 Component

### TransformComponent

entity들이 어디에 위치하는가

![](./transform-comp.png)

### SpriteRendererComponent

entity가 어떻게 그려질지를 정함

![](./sprite-renderer-comp.png)



### 타일

#### 발판

* 캐릭터가 오를 수 있는, 밟을 수 있는 타일을 표시



세팅 -> 만들기 -> 발판

풋홀드가 있는 모델들을 edit foothold 를 사용해 풋홀드 위치를 변경할 수 있음

마찰력 속도등 풋홀드 속성 존재



### 맵 레이어

하나의 맵에 최대 10개의 레이어 사용 가능



### 충돌 감지

#### TriggerComponent



* 핸들러를 사용해서 충돌시 액션 설정
