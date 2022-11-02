---
title: "Java - new ArrayList<>() vs Arrays.asList() vs List.of()"
date: 2022-11-02
tags:
  - java
---

## Arrays.asList()

```java
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```

인자가 `T . . .`이므로 모든 자료형의 파라미터를 여러 개 받을 수 있다.

<br/>

```java
List<Integer> list1 = Arrays.asList(1, 2, 3);
List<String> list2 = Arrays.asList("3", "6", "9");

String[] arr = {"1", "2"};
List<String> list3 = Arrays.asList(arr);
```

<br/>

`asList`라는 이름을 보고 인자를 ArrayList로 변환해주는 함수인가 했는데 ArrayList가 아닌 다른 클래스를 반환해 준다.

```java
import java.util.ArrayList; // new ArrayList<>()
import java.util.Arrays; // Arrays.asList()
```



## new ArrayList<>() 와 Arrays.asList()의 차이

그럼 둘의 차이는 무엇일까?<br/>

* `Arrays.asList()`는 크기가 고정된 리스트를 반환하며, 원소의 추가, 삭제가 불가능하다.
* `new ArrayList<>()`는 원소의 추가, 삭제가 가능하다.

`Arrays.asList()`는 `set`메서드를 통해 특정 인덱스의 값을 변경시킬 수 있고, 인덱스에 접근해서 값을 변경할 수도 있다.



### 📌 new ArrayList<>()를 사용하면 편한 경우

* 컬렉션 생성 시, 새로운 주소값으로 할당하기 위한 용도



### 📌 Arrays.asList()를 사용하면 편한 경우

* 테스트 케이스와 같이 요소의 개수가 제한되어 사용되는 경우

배열의 크기가 변하면 안되거나 변하지 않을 경우 사용한다. 

```java
// 3 6 9 게임에서 3, 6, 9를 파싱하기 위한 배열 - 변하지 않는 배열
List<String> target = Arrays.asList("3", "6", "9");
```

```java
// 테스트 케이스
@Test
void Test() {
   List<Car> cars = Arrays.asList(new Car("K2"), new Car("Sonata")); 
   ...
}
```



### 📌 Arrays.asList vs List.of

위 `Arrays.asList()`의 예시에서 테스트 케이스가 나왔다. 그러나 테스트 케이스에 사용하는 array의 경우`Arrays.asList()`외에 ` List.of`로 할당받는 경우가 많다. 그럼, 둘의 차이는 무엇일까? 간략하게 나타내면 아래 표와 같다.

|                   | new ArrayList<> () | Arrays.asList() | List.of() |
| ----------------- | ------------------ | --------------- | --------- |
| 삽입 add          | O                  | X               | X         |
| 삭제 remove       | O                  | X               | X         |
| 변경 set, replace | O                  | O               | X         |
| Null 허용         | O                  | O               | X         |

* 상세한 설명은 https://kim-jong-hyun.tistory.com/31를 참고하면 좋을 것 같다.



## 참고

* https://tecoble.techcourse.co.kr/post/2020-05-18-ArrayList-vs-Arrays.asList/
* https://kim-jong-hyun.tistory.com/31
