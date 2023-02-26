---
title: "TDD해본 후기"
date: 2023-02-25
update: 2023-02-25
tags:
  - 우테코
  - wooteco
  - 우아한테크코스
series: "wooteco"
---



## TDD란?

> 테스트를 먼저 작성하면서 리팩토링을 진행하는 방법

우테코의 두 번째 미션은 사다리 게임을 구현하는 것이었다. 이번 미션에는 특별한 요구사항이 있었는데 바로 `TDD`방식으로 구현을 하는 것이었다. 인생 처음으로 `TDD`를 해보았는데 테스트 코드를 먼저 작성하고 그 다음에 프로덕션 코드를 작성하는 것이 너무 어려웠다. 😭 <br/>

그래도 직접 `TDD`를 해보면서 `TDD`를 왜 해야하는지를 몸소 느낄 수 있었다. `TDD`를 하며 느낀 바를 정리해보고자 한다.



### 📌 TDD 그거 어떻게 하는건데!

#### TDD의 원칙

- `원칙 1` - 실패하는 단위 테스트를 작성할 때까지 프로덕션 코드(production code)를 작성하지 않는다.
- `원칙 2` - 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
- `원칙 3` - 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.





![](tdd-cycle.jpeg)

#### 1️⃣ 실패하는 코드 작성하기

- 무작정 테스트 코드부터 작성한다!

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

>  `Name`이라는 객체가 없는 상태에서 테스트 코드부터 작성했기에 컴파일 에러가 발생함!

<br/>

#### 2️⃣ 테스트가 성공하게 바꾸기

* 테스트 코드의 컴파일이 성공할 수 있게 객체와 메소드를 생성한다.

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

* 테스트가 성공할 수 있게 최소한으로 프로덕션 코드를 수정한다.

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

* 테스트 코드가 성공적으로 수행됐다면, 코드 리팩토링을 시행한다.

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





### 📌 왜 TDD를 해야할까?

- 디버깅 시간을 줄여준다.
- 동작하는 문서 역할을 한다.
- 도메인에 대한 이해도를 높일 수 있다.
- 변화에 빠르게 대응할 수 있는 연습을 할 수 있다.
- 과도한 설계에 따른 추가 비용을 해소한다.

TDD를 하면서 느낀 장점은 도메인의 책임을 명확하게 만들어준다는 것이었다. 테스트코드를 작성하기 위해 도메인의 메소드가 어떤 `input`을 받아 어떤 `output`을 반환해야 하는지를 미리 명시하고 그에 맞게 프로덕션 코드를 짜니 도메인이 가지는 책임이 명확해졌다.<br/>

또한 리팩토링 중 도메인 로직의 내부 구현이 변경되더라도, 기존에 작성한 테스트 코드를 통해 핵심 로직이 잘 작동되는지를 테스트할 수 있으니 리팩토링에 대한 불안함이 줄었다.<br/>

그리고 이전의 나는 프로덕션 코드를 먼저 작성한 다음 테스트 코드를 작성했는데, 프로덕션 코드가 완성이 된 상태에서 테스트 코드를 작성하는 일은... 매우 귀찮았다...😊 이미 코드가 잘 동작하는 것을 확인한 다음에 테스트를 작성하니 감동도 없고 재미도 없었다. TDD를 하면서 테스트 코드를 먼저 작성하게 되니 테스트가 통과할때마다 감동과 재미를 느낄 수 있었다...😅 



### 📌 TDD를 하면서 어려웠던 점

* `모든 기능을 TDD로 구현해 단위 테스트가 존재해야 한다.`

이번 미션의 요구 사항이다. 모든 기능에 대한 테스트를 먼저 작성하고 프로덕션 코드를 작성하다보니... 불만이 생겼다. 테스트를 해보고 싶은 기능도 존재하지만, 테스트를 굳이 하지않아도 불안하지않는 기능들도 존재했다. 이런 경우는 꾸역꾸역 테스트를 작성하는 느낌이라 테스트를 작성하는 시간이 아깝기도 했다.<br/>

또, 테스트 코드를 일부 작성했을 때, 설계에 변화가 생기면 프로덕션 코드를 포함해 테스트 코드까지 전부 고치느라 시간이 많이 들었다. <br/>

물론 이건 내가 큰 규모의 프로그램을 개발해본 적이 없기에 TDD의 중요성을 잘 몰라서, 테스트 코드 작성전에 프로그램 설계를 잘못해서 느끼는 불만이겠지만 TDD를 하지 않았던 이전 미션보다 TDD를 진행한 이번 미션이 개발하는데 더 시간이 오래 걸리고, 고민할 여지가 많았던 것은 명백하다. 개발자는 비용적 측면을 고려하여 TDD를 진행할지, 만약 TDD를 한다면 얼마나 자세히 테스트를 해야할 지를 항상 고민해야할 것 같다.

