---
title: "우아한 테크 코스 백엔드 2주차 회고"
date: 2023-02-15
update: 2023-02-15
tags:
  - 우테코
  - wooteco
  - 우아한테크코스
series: "wooteco"
---



## 2주차에서는 무엇을 배웠을까?

2주차부터는 새로운 미션인 `사다리 게임`이 시작됐다! 이번 미션은 `TDD` 를 하는것이 요구사항이었다. `TDD`를 하면 무엇이 좋을까? 2주차동안 `TDD`에 대해 배운점과 느낀점을 기록해보려고 한다. 😀



## TDD

>  테스트를 먼저 작성하면서 리팩토링을 진행하는 방법



### 📌 TDD의 장점

* 디버깅 시간을 줄여준다.
* 동작하는 문서 역할을 한다. (인풋과 아웃풋이 명확)
* 도메인에 대한 이해도를 높일 수 있다.
* 변화에 빠르게 대응할 수 있는 연습을 할 수 있다.
* 과도한 설계에 따른 추가 비용을 해소한다.



### 📌 TDD 원칙

- 원칙 1 - 실패하는 단위 테스트를 작성할 때까지 프로덕션 코드(production code)를 작성하지 않는다.
- 원칙 2 - 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
- 원칙 3 - 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.



### 📌 TDD 어떻게 하면 될까?

![](tdd-cycle.jpeg)

#### 1️⃣ 실패하는 코드 작성하기

* 무작정 테스트를 작성한다! 

```java
public class NameTest(){
    @Test
    public void 이름_길이_예외던지기(){
        assertThatThrownBy(() -> new Name("aaaaaa"))
            .isInstanceOf(IllegalArgumentException.class)
                .hasMessage("이름은 1자 이상 5자이하만 입력할 수 있습니다.");
    }
}
```

```java
public class Name(){
    private String name;
    
    public Name(String name){
        this.name = name;
    }
}
```

> `Name` 객체에 유효성을 체크하는 함수가 없어서 실패! 

<br/>

#### 2️⃣ 테스트가 성공하게 바꾸기

```java
public class Name(){
    private String name;
    
    public Name(String name){
        if(nams.size() >= 5 || name.size() < 1){
            throw new IllegalArgumentException("이름은 1자 이상 5자이하만 입력할 수 있습니다.");
        }
        this.name = name;
    }
}
```

<br/>



#### 3️⃣ 리팩토링

```java
public class NameTest(){
    @ParameterizedTest()
    @ValueSource(strings = {"aaaaaa", "", "1234567"})
    public void 이름_길이_예외던지기(String name){
        assertThatThrownBy(() -> new Name(name))
            .isInstanceOf(IllegalArgumentException.class)
                .hasMessage("이름은 1자 이상 5자이하만 입력할 수 있습니다.");
    }
}
```



```java
public class Name(){
    private static final int LENGTH_LOWER_BOUND = 1;
    private static final int LENGTH_UPPER_BOUND = 5;
    private String name;
    
    public Name(String name){
        validateLength(name);
        this.name = name;
    }
    
    private void validateLength(String name){
        if(nams.size() >= LENGTH_LOWER_BOUND || name.size() < LENGTH_UPPER_BOUND){
            throw new IllegalArgumentException("이름은 1자 이상 5자이하만 입력할 수 있습니다.");
        }
    }
}
```

> 리팩토링을 하면 기존 테스트 코드가 깨지는 것은 아닐까? 
>
> 그건 리팩토링이 아님. 기능의 변화가 일어나면 안됨! 코드의 퀄리티를 높이는 것이 TDD의 리팩토링

<br/>

**작은 단위부터 테스트를 시작해서 도메인에 대한 이해도가 높아지면 그 때 테스트의 범위를 넓힌다.**



#### 답해보기

- 내가 TDD, 리팩토링을 하는 이유는 무엇인가?
- 기존에 구현하는 방식과 TDD로 코드를 구현할 때 어떠한 차이를 느꼈는가?
- 미션을 진행하면서 리팩토링을 경험하며 어떠한 어려움을 겪었는가?
- 리팩토링 과정에서 어려움을 줄이기 위해 어떠한 시도를 해볼 수 있는가?




