---
title: "Java - 람다식(Lambda expression)"
date: 2022-09-01
tags:
  - java
series: "java-programming"
---



## 함수형 프로그래밍

> 함수의 구현과 호출만으로 프로그래밍이 수행되는 방식<br/>
>
> 자바 8부터 함수형 프로그래밍 방식을 지원하고 이를 람다식이라 함

외부의 변수를 사용하지 않는 함수, 즉 외부 자료에 부수적인 영향(side effect)를 주지 않도록 구현하는 방식이다. 때문에 동시에 여러 함수가 실행되도 문제가 생기지않음 - 병렬처리 가능<br/>

함수형 프로그래밍은 함수의 기능이 자료에 독립적임을 보장한다. 즉, 다양한 매개변수에 대해서 동일한 기능을 제공할 수 있다.



## 람다식 문법

* 일단 함수 이름이 없다. (익명 함수)
* 구조는 매개변수와 매개변수를 이용한 실행문 이렇게 두 개가 있다.
  * (매개변수) -> {실행문;}



### 📌 예제1

```java
// add 예시
(int x, int y) -> {return x+y;}
```

<br/>

* 사실 자료형을 그냥 생략할 수 있다.

```java
 (x, y) -> {return x+y}
```

<br/>

* 매개변수가 하나라면 소괄호를 생략할 수도있다.

```java
str->{System.out.println(str);}
```

<br/>

* 실행문이 한 문장인 경우 중괄호 생략 가능하다.

```java
str -> System.our.println(str);
```

<br/>

* 실행문이 한 문장이더라도 return문은 중괄호를 생략할 수 없다.

```java
str -> {return str.length();}
```

<br/>

* 실행문이 한 문장의 반환문인 경우엔 return과 중괄호를 모두 생략할 수 있다.

```java
(x,y) -> x+y;
str -> str.length; // 이게 되네
```



### 📌 예제2 - 함수형 인터페이스

```java
@FunctionalInterface // 주석을 안넣어도 일단 돌아가지만 안넣으면 다른 오류가 생길수도 있다고한다.
public interface AddInterFace {
    public int addMethod(int x, int y);
}
```

```java
public class AddTest{
    public static void main(String[] args){
        AddInterFace add = (x, y) -> {return x+y;};
        // 자바는 객체없이 메서드가 호출될 수 없다.
        // 그러므로 실질적으로 람다식내부에 익명 내부 클래스가 새성되고 익명 객체가 만들어진다.
        // 익명 내부 클래스와 마찬가지로 람다식 내부에서도 외부에 있는 지역 변수의 값을 변경하면 오류가 발생한다.
        add.addMethod(3,5) // 출력하면 8
    }
}
```

* 인터페이스를 생성하고 그 안에 미리 람다식으로 구현할 메서드를 선언해준 이유는 .java파일은 함수(람다식) 하나만 존재할 수 없다.
  * 무조건 클래스나 인터페이스가 존재한다.
* 여기서 인터페이스 AddInreFace는 abstract 메서드를 단 하나만 가지고 있어야한다. 이런 인터페이스를 함수형 인터페이스라고한다.



## 객체 지향 프로그래밍 vs 람다식 구현

문자열 두 개를 연결하여 출력하는 예제를 두 가지 방식으로 구현해보자.

```java
// 객체 지향 프로그래밍
public class StringConCatImpl implements StringConCat{
    @Override
    public void makeString(String s1, String s2){
        System.out.println(s1+s2);
    }
}
```

```java
// 함수형 인터페이스 for 람다식
public interface StringConCat {
    public void makeString(String s1, String s2);
}
```

```java
// 실행 코드
public class StringConcatTest {
	public static void main(String[] args) {

		String s1 = "Hello ";
		String s2 = "World";
        
        // 객체 지향 프로그래밍
		StringConCatImpl concat1 = new StringConCatImpl();
		concat1.makeString(s1, s2);
        
        // 람다식
        StringConCat concat2 = (s1, s2) -> System.out.println(s1+s2);
        concat2.makeString(s1, s2);
    }
}

```

> 가독성은 객체 지향 프로그래밍이 좋지만 확실히 인터페이스에서는 구현을 안해도 되니 람다식의 코드가 훨씬 적긴하다. 



## 변수처럼 활용될 수 있는 람다식

또한 람다식은 장점은 여기서 끝이 아닌데...(두근)<br/>

함수를 변수처럼 사용될 수 있다는 장점이 있다.



### 😀 변수의 특징

* 특정 자료형으로 변수를 선언한 후 값을 대입할 수 있음 (int a = 1;)
* 매개 변수로 전달하여 사용 가능함 (int add(int x, int y))
* 메서드 반환값으로 내보낼 수 있음 (return num;)

> 람다식을 활용하면 위 세가지 특징을 함수에 적용할 수 있다!



### 📌 함수를 변수에 대입

```java
interface ShowString{
    void showString(String str);
}
```

대충 위에 같은 인터페이스가 있다고 하자.

<br/>

```java
ShowString lambdaStr = str -> System.out.println(str);
lambdaStr.showString("lambda!") // output : lambda!
```

> 위에서 나온 방식이지만 인터페이스형 변수에 람다식을 대입할 수 있다.



### 📌 함수를 매개변수로 전달

```java
// 이런 함수가 있을 때
public static void showMyString(PrintString p) {
	p.showString("lambda!");
}

// 람다식을 매개 변수로 전달 가능
ShowString lambdaStr = str -> System.out.println(str);
showMyString(lambdaStr); // output : lambda!
```



### 📌 함수를 반환 값으로 사용

```java
public static ShowString returnString(){ // 반환값이 인터페이스 ShowString
    return str->System.out.println(str);
}

ShowString returnStr = returnString();
returnStr.showString("lambda!"); // output : lambda!
```





## 회고(라기엔 그냥 잡담)

호... 코드를 짧게 쓸 수 있는 람다식을 배웠지만 아직 응애컴공이라 그럴까? 객체 지향 방식이 가독성있고 더 좋다 ㅋㅋㅋ ㅠ <br/>

사용하다보면... 익숙해지겠지? 가끔 고수분들 코드보면 람다식 기깔나게 쓰시던데 나도 따라하고 싶다... ㅎ 고수 되는 그 날까지 ! 화이팅...~

<br/>
