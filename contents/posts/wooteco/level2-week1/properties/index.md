---
title: "테스트용 프로퍼티 파일 생성하기"
date: 2023-04-16
update: 2023-04-16
tags:
  - 우테코
  - wooteco
  - 우아한테크코스
series: "wooteco"

---

## 테스트용 프로퍼티 파일 생성하기

`src/main/resources` 에 `application.properties` 파일이 있다. 이 프로퍼티 파일은 개발용 어플리케이션에 대한 프로퍼티 설정을 수행한다.



그러나 테스트용으로 프로퍼티 파일을 따로 적용하고 싶다면 어떻게 해야 할까?

만약 개발용에서 사용하는 데이터베이스 테이블과 테스트용에서는 사용되는 데이터베이스 테이블이 다르다면 프로덕션 코드와 테스트 코드는 각각 다른 프로퍼티 파일이 필요하다.

테스트용 프로퍼티 파일을 어떻게 만들면 좋을지 알아보자.



## @ActiveProfiles

`src/main/resources` 패키지 아래에 프로덕션용으로 사용할 `application.properties`파일과 테스트용으로 사용할 `application-test.properties` 파일을 생성한다.

<br/>

#### 프로덕션용 프로퍼티 파일

```properties
# src/main/resources/application.properties

value="mainValue"
```

<br/>



#### 테스트용 프로퍼티 파일

```properties
# src/main/resources/application-test.properties

value="testValue"
```

<br/>



`@ActiveProfiles`를 사용하면 `application-{name}.properties`파일의 `name`부분을 인자로 넣어 원하는 프로퍼티 파일을 적용할 수 있다.

```java
// src/test/java/org

@SpringBootTest
@ActiveProfiles("test") // application-test.properties 파일이 적용된다
public class MainTest {
    @Value("${value}")
    private String value;

    @Test
    void test() {
        System.out.println(value);
    }
}

// 출력 : testValue
```







## 패키지 분리

`src/test/resources` 패키지의 아래에 프로퍼티 파일을 생성하면, 테스트 코드가 `src/test/resources`에 있는 파일을 우선적으로 적용한다.

<br/>

#### 프로덕션용 프로퍼티 파일

```properties
# src/main/resources/application.properties

value="mainValue"
```

<br/>



#### 테스트용 프로퍼티 파일

```properties
# src/test/resources/application.properties

value="testValue"
```

<br/>



```java
// src/test/java/org

@SpringBootTest
public class MainTest {
    @Value("${value}")
    private String value;

    @Test
    void test() {
        System.out.println(value);
    }
}

// 출력 : testValue
```

