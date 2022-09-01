---
title: "Java - String, StringBuilder, StringBuffer, textblock"
date: 2022-09-01
tags:
  - java
series: "java-programming"
---

## String 클래스

```java
String str1 = new String("abc");
String str2 = "abc";
// new와 "" 두 가지 방식으로 선언 가능
```

* 힙 메모리에 인스턴스로 생성되는 경우와 상수 풀(constant pool)에 있는 주소를 참조하는 두 가지 방법
* 힙 메모리는 생성될때마다 다른 주소 값을 가지지만, 상수 풀의 문자열은 모두 같은 주소 값을 가짐

<br/>

```java
String str1 = new String("abc");
String str2 = new String("abc");
		
System.out.println(str1 == str2); // false

String str3 = "abc";
String str4 = "abc";

System.out.println(str3 == str4); //true

```

> new를 사용한 방법은 인스턴스를 생성하는 방법이므로 힙 영역에 따로 생기기에 값이 같더라도 다른 객체이다.<br/>
>
> ""를 사용한 방법은 상수 풀에 있는 주소를 참조하기 때문에 같은 값이라면 동일한 주소를 참조한다.



### 🤔 String값을 변경하고 싶다면?

한번 생성된 String은 불변(immutable)하다. 그렇다면 어떻게 값을 바꿀 수 있을까?

* String을 연결하면 기존의 String에 연결되는 것이 아닌 새로운 문자열이 생성된다. 
* str1 = str1.concat(str2); 하더라도 새로운 String에 str1과 str2를 붙여 문자열을 만든뒤 str1이 가리키는 주소를 새 String으로 연결하는 것이다. 
* 이런 방법은 메모리 낭비가 발생한다.
* 이를 해결하기 위해 StringBuilder와 StringBuffer를 사용하면 된다.



## StringBuilder, StringBuffer 활용하기

* 내부적으로 가변적인 char[]를 멤버 변수로 가짐
* 문자열을 여러번 연결하거나 변경할 때 사용하면 유용함
* 새로운 인스턴스를 생성하지 않고 char[] 를 변경함
* toString() 메서드로 String반환

>  둘의 차이는 StringBuffer는 멀티쓰레드환경에서 동기화가 가능하다는 것
>
> * 단일 쓰레드 프로그램에서는 StringBuilder,
> * 다중 쓰레드 프로그램에서는 StringBuffer를 사용하면 된다.

<br/>

```java
StringBuilder buffer = new StringBuilder(str1);
buffer.append(str2);
String result = buffer.toString();
```



## text block 사용하기 (java 13)

* 문자열을 """ """ 사이에 이어서 만들 수 있음
* html, json 문자열을 만드는데 유용하게 사용할 수 있음
