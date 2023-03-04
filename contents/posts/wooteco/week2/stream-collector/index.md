---
title: "Stream으로 야무지게 collect하기"
date: 2023-03-05
update: 2023-03-05
tags:
  - 우테코
  - wooteco
  - 우아한테크코스
series: "wooteco"
---

## Stream으로 야무지게 collect해야지~

`Stream`의 `collect()`메소드를 사용하면 연산 결과를 List나 Set등 Collection으로 묶어 반환할 수 있다. `Stream`을 사용해서 객체를 야무지게 모아보자!



## 📌 toList() & toSet()

`collect` 메소드로 가장 많이 사용하는 기능이다ㅎㅎ. Stream의 요소들을 `List`, `Set`로 묶어 반환한다. 

```java
List<String> example = Arrays.asList("aa", "bb", "bb", "cc");
List <String> listResult = example.stream().collect(Collectors.toList()); // aa, bb, bb, cc
List<String> setResult = example.stream().collect(Collectors.toSet()) // aa, bb, cc

```





* ### 방어적 복사

`Collectors`의 `toList()`, `toSet()`은 `addAll()`메소드를 사용해 방어적 복사를 수행한다. 방어적 복사를 수행한 복사본은 원본과 다른 컬렉션 객체를 참조하게 되지만, 객체 내부에 있는 원소들은 원본과 동일한 주소를 참조한다. 예시를 보며 이해를 해보자!

<br/>

* 이름을 저장하는 `Name`이라는 객체가 존재한다. 세 개의 `Name`을 만들어서 리스트를 생성해보자. 

```java
Name n1 = new Name("aa");
Name n2 = new Name("bb");
Name n3 = new Name("cc");

List<Name> names  = new ArrayList<>(Arrays.asList(n1, n2, n3));
List<Name> newNames = names.stream().collect(Collectors.toList());;
```

<br/>

- List 컬렉션 변경

```java
List<Name> names  = Arrays.asList(n1, n2, n3); // aa, bb, cc
List<Name> newNames = names.stream().collect(Collectors.toList()); //aa, bb, cc
names.add(new Name("dd"));

// names : aa, bb, cc, dd
// newNames : aa, bb, cc
```

복사본은 원본과 다른 주소를 가지는 컬렉션 객체이기에, `names`에 변화가 생겨도 `newNames`에 영향을 끼치지 않는다.

<br/>

- List 컬렉션 내부 객체 변경

```java
List<Name> names  = Arrays.asList(n1, n2, n3); // aa, bb, cc
List<Name> newNames = names.stream().collect(Collectors.toList()); //aa, bb, cc

n1.setName("dd");
// names : dd, bb, cc
// newNames : dd, bb, cc
```

객체 내부에 있는 원소들은 원본과 동일한 주소를 참조하므로, 내부 객체의 값이 변경되면 `names`와 `newNames` 모두 변화가 생긴다.

<br/>

이를 피하고 싶다면 `new`로 내부 객체를 새로 생성한뒤 collect하는 방법이 있다.

```java
List<Name> newNames = names.stream().map(iter -> new Name(iter.getName())).collect(Collectors.toUnmodifiableList());
```





## 📌 toMap()

`List`와 `Set` 외에 `Map`으로도 `Stream` 요소들을 모을 수 있다. `toMap`의 매개변수를 보면 다음과 같다!

```java
Collector<T, ?, M> toMap(Function<? super T, ? extends K> keyMapper,
                                Function<? super T, ? extends U> valueMapper,
                                BinaryOperator<U> mergeFunction,
                                Supplier<M> mapSupplier)
```

- `keyMapper` : Map의 key를 만드는 함수
- `valueMapper` : Map의 value를 만드는 함수
- `mergeFunction` : 중복되는 키 값이 들어올 경우 수행되는 함수
- `mapSupplier` : 결과와 함께 비어있는 새 맵의 타입을 반환하는 함수

<br/>

예시를 통해 `toMap`을 어떻게 활용하는지 봐보자!

```java
Map<Name, Integer> nameTable = names.stream().collect(Collectors.toMap(name -> name, name -> name.length());
```

> `nameTable`은 `Name`객체를 key로, key값인 `Name`의 길이를 value로 가지는 `Map`이다. `toMap`의 인자로 key와 value를 만드는 함수를 넣어 `Map`을 생성할 수 있다.

<br/>

- key값이 중복일 경우

```java
Map<Name, Integer> nameTable = names.stream().collect(Collectors.toMap(name -> name,
                name -> name.length(),
                (old, now) -> old));
```

> `mergeFunction`을 인자로 넣어, 어떤 key의 값을 사용할 것인지를 정할 수 있다.

<br/>

- 새 맵의 타입으로 반환하고 싶은 경우

```java
Map<Name, Integer> nameTable = names.stream().collect(Collectors.toMap(name -> name,
                name -> name.length(),
                (old, now) -> old),              					               LinkedHashMap::new);
```

> `mapSupplier` 에 반환하고 싶은 타입의 `Map`을 지정할 수 있다.



## 📌 Unmodifiable Collection

`toUnmodifiableList()`, `toUnmodifiableSet() `, `toUnmodifiableMap`을 사용하면 수정이 불가능한 컬렉션 객체를 반환한다. `Unmodifiable Collection`에 `set()`, `add()`, `addAll()`등의 메소드를 호출하면 `UnsupportedOperationException`이 발생한다.

물론 방어적 복사를 수행하므로, `toUnmodifiableList()`로 생성한 리스트의 내부 객체가 변경될수도 있다는 점을 유의해야한다.

```java
List<Name> result = names.stream().collect(Collectors.toUnmodifiableList());
```

<br/>

## 📌 toCollection()

`toSet`, `toList`, `toMap`은 Collection을 지정할 수 없다.

`toCollection()`을 사용해 다른 타입의 Collection으로 묶는 것이 가능하다.

```java
List<String> result = givenList.stream()
  .collect(toCollection(LinkedList::new))
List<String> result = givenList.stream()
  .collect(toCollection(LinkedList::new))
```

<br/>

## 📌 collectingAndThen()

`collect()`를 완료한 직후 결과에 대해 다른 작업을 수행할 수 있다.

```java
public class Name {
	...
    @Override
    public String toString() {
        return "저는" + name +"입니다!";
    }
}
String result = names.stream().collect(collectingAndThen(toList(), List::toString));
System.out.println(result);

// 실행 결과
// [저는aa입니다!, 저는bb입니다!, 저는cc입니다!]
```

<br/>



## 📌 joining()

Stream 요소가 char이나 String일 경우, 결합할 수 있다.

```java
String result = givenList.stream()
  .collect(joining())
```

<br/>

* 구분자, prefix, suffix 설정도 가능

```java
String result = givenList.stream() //"aa", "vv"
  .collect(joining(" ", "pre", "suf"));
```




