---
title: "멋쟁이사자처럼 X 넥슨 MOD Suppoters Hackathon 5주차 회고"
date: 2022-07-01
tags:
  - mod
series: "mod"
---

## 자주쓰이는 Component

* MovementComponent
  * 점프력과 이동속도를 결정

<br/>

* RigidbodyComponent
  * 과감속에 따른 움직임을 디테일하게 설정할 수 있다.(중력 등 물리와 관련된)
  * 엔티티의 block 여부도 결정할 수 있음. (block이 활성화되면 해당 Entity를 캐릭터가 통과할 수 없음)
  * 쿼터뷰 설정도 가능

<br/>

* TriggerComponent
  * 충돌 트리거 발생

<br/>

* WebSpriteComponent
  * 웹 디스플레이 설정
  * 애니메이션도 가능

<br/>

* YoutubePlayerComponent
  * 유투브 영상을 디스플레이





### Trigger & Movement 예시 - 먹으면 이동속도가 증가하는 아이템

캐릭터가 먹으면 속도가 증가하고 점프력이 높아지는 아이템을 만들어보자!

> 해당 기능을 위해선 캐릭터가 아이템을 먹는걸 감지하는 `Trigger Componet`와 트리거가 발생하면 어떻게 처리할 것인지를 결정하는 추가 컴포넌트 `ItemComponent` 가 필요하다.

<br/>

* `Workspace`의 `Mydesk`에서 `ItemComponent`를 생성하고 아래와 같이 내용을 작성한다.

![](./item-component.png)

> 이벤트 핸들러를 추가하고, <br/>
>
> TriggerBodyEntity의 Movement 정보를 가져와 캐릭터의 속도(InputSpeed)와 점프력(JumpForce)를 상승시킨다.<br/>
>
> * movement가 없는 Entity가 충돌 될 수도 있으므로 예외처리를 해준다.
>
> Enable을 false로 만들어 아이템을 먹으면 아이템이 사라지게 만든다.

<br/>

* Item Entity에  `TriggerComponent`와 방금 만든 `ItemComponent`를 추가한다.

![](./scene.png)

<br/>

<br/>

여기까지하면 구현이 끝났다! 결과를 확인해보자

* 아이템 먹기 전 점프력

![](./before.png)

<br/>

* 아이템 먹은 후 점프력

![](./after.png)




