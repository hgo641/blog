---
title: "flex 해보기 ㅎㅎ"
date: 2022-04-02
tags:
  - css
---

### container와 item

flex하기전 알아야할 컨테이너와 아이템
컨테이너가 전체 박스
아이템이 컨테이너 안에 있는 요소

flex를 사용해서 정렬해주려면 컨테이너에

```css
display: flex;
```

속성을 추가해준다.

### 주축

```css
flex-direction: row;
```

flex-direction속성으로 flex의 주축을 설정할 수 있다

- row
- row-reverse
- column
- column-reverse

### 주축을 기준으로 정렬할때는 justify-content를 사용

```css
justify-content: flex-end;
```

- flex-start
- center
- flex-end
- space-between : 맨 끝 간격제로
- space-around : 맨 끝 간격있음

### 반대축을 기준으로 정렬할때는 align사용

- align-items
- align-content - wrap을 했을때

### wrap

```css
flex-wrap: wrap;
```

- wrap
- wrap-reverse  
  wrap을 하면 item이 공간을 벗어날때 다음 줄에 이어서 나오게한다.
