---
title: "Java - Class 클래스와 동적로딩"
date: 2022-09-01
tags:
  - java
series: "java-programming"
---

## Class 클래스

* 자바의 모든 클래스와 인터페이스는 컴파일 후 class 파일이 생성됨
* Class 클래스는 컴파일 된 class 파일을 로드하여 객체를 동적 로드하고, 정보를 가져오는 메서드가 제공됨
* Class.forName("클래스 이름") 메서드로 클래스를 동적으로 로드 함

```java
// 예시
Class c = Class.forName("java.lang.String");
```





### 📌 동적 로딩

- 컴파일 시에 데이터 타입이 binding 되는 것이 아닌, 실행(runtime) 중에 데이터 타입을 binding 하는 방법
- 프로그래밍 시에는 문자열 변수로 처리했다가 런타임시에 원하는 클래스를 로딩하여 binding 할 수 있다는 장점
- 컴파일 시에 타입이 정해지지 않으므로 동적 로딩시 오류가 발생하면 프로그램의 심각한 장애가 발생가능



### 📌 Class의 newInstance()메서드로 인스턴스 생성

- new 키워드를 사용하지 않고 클래스 정보를 활용하여 인스턴스를 생성할 수 있음



### 📌 클래스 정보 알아보기

- reflection 프로그래밍 : Class 클래스를 사용하여 클래스의 정보(생성자, 변수, 메서드)등을 알 수 있고 인스턴스를 생성하고, 메서드를 호출하는 방식의 프로그래밍
- 로컬 메모리에 객체 없는 경우, 원격 프로그래밍, 객체의 타입을 알 수 없는 경우에 사용
- java.lang.reflect 패키지에 있는 클래스를 활용하여 프로그래밍
- 일반적으로 자료형을 알고 있는 경우엔 사용하지 않음

<br/>

```java
public class Person {
	private String name;
	private int age;
	
	public Person() {};
	
	public Person(String name) {
		this.name = name;
	}
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
    
    public String toString() {
		return name+", "+age;
	}
}
```

> 위와 같이 name과 age멤버변수를 지니며 생성자가 세 개 있는 클래스 Person이 있다고 해보자



```java
Class c1 = Class.forName("chap2.Person"); // Person 클래스를 반환
Person person1 = (Person)c1.newInstance(); // 인스턴스를 생성하고 Person으로 다운캐스팅
System.out.println(person1); // null, 0

Constructor cons = c1.getConstructor(); // 생성자를 가져옴
Person personLee = (Person)cons.newInstance(); // 가져온 생성자로 인스턴스 생성
System.out.println(personLee); // null, 0
```

> * forName 메서드를 통해 클래스를 가져오고, newInstance를 통해 가져온 클래스로 인스턴스를 생성할 수 있다.
> * 인스턴스 명을 출력하면 toString()의 반환값이 출력된다.
> *  newInstance()는 Object 타입을 반환하므로 다운캐스팅이 필요하다.



<br/>

```java
Class[] paramTypes = {String.class, int.class}
Constructor cons = c1.getConstructor(paramTypes); // 생성자를 가져옴
Object initargs = {"홍길동", 20}
Person personLee = (Person)cons.newInstance(initargs); // 가져온 생성자로 인스턴스 생성
System.out.println(personLee); // 홍길동, 20
```

> * 생성자의 파라미터 타입을 Class 배열에 담아 해당 파라미터들을 요구하는 생성자를 불러올 수도 있다.
> * 위 방식으로 불러온 생성자에 들어갈 매개변수는 Object 형태로 전달할 수 있다.
