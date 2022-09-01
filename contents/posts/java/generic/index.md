---
title: "Java - 짜잔~ 무엇이든 담을 수 있는 제네릭이지요~"
date: 2022-09-01
tags:
  - java
series: "java-programming"
---

## Generic 자료형

* 클래스에서 사용하는 변수의 자료형이 여러개 일수 있고, 그 기능(메서드)은 동일한 경우 클래스의 자료형을 특정하지 않고 추후  해당 클래스를 사용할 때 지정 할 수 있도록 선언하는 자료형이다.
* 실제 사용되는 자료형의 변환은 컴파일러에 의해 검증되므로 안정적인 프로그래밍 방식이다.
* 컬렉션 프레임워크에서 많이 사용되고 있음



### 📌 예시 - 제네릭 사용 x

```java
public class DataBase {

	private Object type;

	
	public Object getType() {
		return type;
	}

	public void setType(Object type) {
		this.type = type;
	}
	
}
```

```java
public class MySqlDataBase {
	public void description() {
		System.out.println("mysql을 사용합니다.");
	}
}
```

```java
public class OracleDataBase {
	public void description() {
		System.out.println("oracle을 사용합니다.");
	}
}
```

```java
public class DataBaseTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		DataBase db = new DataBase();
		MySqlDataBase mysql = new MySqlDataBase();
		db.setType(mysql);
		// DataBase database = db.getType(); error. db.type is Object
		MySqlDataBase database = (MySqlDataBase)db.getType();
		database.description();
	}

}
```

> Generic없이 DataBase의 type을 Object로 해두고 상황에 따라 두 가지 타입의 type을 할당하는 방법도 있다.<br/>
>
> 그러나 할당한 type을 getter를 사용해 다시 가져오려면 Object 타입이 반환되기 때문에 다운캐스팅이 필요하다.<br/>
>
> Generic을 사용하면 보다 간편하게 구현할 수 있다.

<br/>

### 📌 예시 - 제네릭 사용 o

```java
public class DataBase<T> {

	private T type;

	
	public T getType() {
		return type;
	}

	public void setType(T type) {
		this.type = type;
	}
	
}
```

* 제네릭 클래스는 클래스명 옆에 <> 다이아몬드 연사자를 붙인다.
* <> 안의 이름은 무엇을 넣든 상관없지만 보편적으로 T를 넣는다.
  * 자료형 매개변수 T(type parameter)
  * E : element, K: key, V : value 등 여러 알파벳을 의미에 따라 사용 가능

<br/>

```java
public class DataBaseTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		DataBase<MySqlDataBase> db = new DataBase<>();
		db.setType(new MySqlDataBase());
		MySqlDataBase database = db.getType();
		database.description();
	}

}
```

* 선언할 때는 <>안에 사용할 type을 선언한다.
* 다운캐스팅할 필요 없이 type을 바로 get할 수 있다.



## T extends 클래스 사용하기

```java
public class DataBase<T extends DataType>{
    ...
}
```

* T 자료형의 범위를 제한할 수 있다. (T에는 특정 클래스를 상속받은 타입만이 들어갈 수 있다.)
* 상위클래스에서 선언하거나 정의하는 메서드를 활용할 수 있다.
* 상속을 받지 않은 경우, T는 Object로 변환되어 Object클래스가 기본으로 제공하는 메서드만 사용가능하다.



## 제네릭 메서드

* 자료형 매개변수(ex. T)를 메서드의 매개변수나 반환 값으로 가지는 메서드는 자료형 매개 변수가 하나 이상인 경우도 있음
* 제네릭 클래스가 아니어도 내부에 제네릭 메서드는 구현하여 사용 할 수 있음
* public <자료형 매개 변수> 반환형 메서드 이름(자료형 매개변수.....) { }

```java
// 정의
public <T, V> void GenericMethod(T t1, V v1){
    ...
}

// 선언
instance.<Interger, String>GenericMethod(t1, v1);
```

```java
// 예시
public class GenericTest {
	public static <T, V> void genericMethod(T t1, V v1) {
		System.out.print(t1.toString());
		System.out.print(v1.toString());
	}
	
	public static void main(String[] args) {
		String str = "hello\n";
		Number i = 10;
		GenericTest.<String,Number>genericMethod(str, i);
	}

}

// 출력
// hello
// 10
```

