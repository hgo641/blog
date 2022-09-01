---
title: "Java - 내부클래스(inner class)"
date: 2022-09-01
tags:
  - java
series: "java-programming"
---

## 내부클래스(inner class)

- 클래스 내부에 선언한 클래스
- 보통 다른 외부클래스에서 사용할 일이 거의 없는 경우에 생성함
- 중첩클래스라고도함
- 종류는 인스턴스 내부 클래스, 정적(static) 내부 클래스, 지역(local) 내부 클래스, 익명(anonymous) 내부 클래스가 있음

### 📌 인스턴스 내부 클래스

- 내부적으로 사용할 클래스를 선언 (private으로 선언하는 것을 권장)

- 외부 클래스가 생성된 후 생성됨 ( 정적 내부 클래스와 다름 )

  > 그렇기에 내부클래스 안에서 static을 생성할 수 없음

- private이 아닌 내부 클래스는 다른 외부 클래스에서 생성할 수 있음

```java
class OutClass {

	private int num = 10;
	private static int sNum = 20;
	private InClass inClass;

	public OutClass(){
		inClass = new InClass(); // 내부 클래스 생성
	}

	class InClass{

		int inNum = 100;
		//static int sInNum = 200;  // error

		void inTest(){
			System.out.println("OutClass num = " +num + "(외부 클래스의 인스턴스 변수)");
			System.out.println("OutClass sNum = " + sNum + "(외부 클래스의 스태틱 변수)");
			System.out.println("InClass inNum = " + inNum + "(내부 클래스의 인스턴스 변수)");
		}

	    //static void sTest(){  //에러 남

	    //}

	}

	public void usingClass(){
		inClass.inTest(); //내부 클래스 변수를 사용하여 메서드 호출하기
	}
}

// test
public class InnerTest {

	public static void main(String[] args) {
		OutClass outClass = new OutClass();
		outClass.usingClass();    // 내부 클래스 기능 호출

		OutClass.InClass inClass = outClass.new InClass();   // 외부 클래스를 이용하여 내부 클래스 생성, 바로 new해서 만들 수 는 없음
		inClass.inTest();
	}

}

```

### 📌 정적 내부 클래스

- 외부 클래스 생성과 무관하게 사용할 수 있음
- 정적 변수, 정적 메서드 사용
- 내부의 멤버 변수, 메서드와 외부클래스의 static 변수, 메서드가 사용가능하다.

```java
OutClass.InStaticClass staticClass = new OutClass.InStaticClass();
```

### 📌 지역 내부 클래스

- 지역 변수와 같이 메서드 내부에서 정의하여 사용하는 클래스
- 메서드의 호출이 끝나면 메서드에 사용된 지역변수의 유효성은 사라짐
- 메서드 호출 이후에도 사용해야 하는 경우가 있을 수 있으므로 지역 내부 클래스에서 사용하는 메서드의 지역 변수나 매개 변수는 final로 선언됨

```java
class Outer{
	int outNum = 100;
	static int sNum = 200;

    // Runnable은 자바에서 제공하는 클래스로 클래스를 쓰레드화 시킨다.
    Runnable getRunnable(int i){ // Runnable 객체를 반환
        int num = 1;

        // 지역 변수처럼 함수 안에서 선언된다.

        class MyRunnable implements Runnable{
            int localNum = 10;

            @Override
            public void run(){
                // num = 999; 사용은 되지만 값을 바꾸는건 에러가 남
                // i = 999; 마찬가지
                System.out.println("i =" + i);
				System.out.println("num = " +num);
				System.out.println("localNum = " +localNum);

				System.out.println("outNum = " + outNum + "(외부 클래스 인스턴스 변수)");
				System.out.println("Outter.sNum = " + Outer.sNum + "(외부 클래스 정적 변수)");

                return new MyRunnable();
            }
        }
    }

}

public class LocalInnerTest {

	public static void main(String[] args) {

		Outer out = new Out();
        Runnable runner = out.getRunnable(10);
        runner.run();
	}
}

```

> **매개변수와 지역변수 사용이 안되는 이유** <br/>
>
> - getRunnable이 호출되는 시점과 내부 class생성주기가 다르기 때문
> - getRunnable은 호출되고 나서 끝이 나면 stack영역에 있는 int i와 int num은 사라진다.
> - 그러나 메서드 run은 나중에 또 호출이 될 수도 있다. run이 호출될 때 i와 num이 없을 수 있으므로 에러가 난다. 즉 stack영역의 변수는 값을 변경할 수 없다.
> - 그래서 그냥 컴파일러가 final int i와 final int num으로 자동으로 변경해준다.

### 📌 익명 내부 클래스

- 이름이 없는 클래스 (위 지역 내부 클래스의 MyRunnable 클래스 이름은 실제로 호출되는 경우가 없음)
- 클래스의 이름을 생략하고 주로 하나의 인터페이스나 하나의 추상 클래스를 구현하여 반환
- 인터페이스나 추상 클래스 자료형의 변수에 직접 대입하여 클래스를 생성하거나 지역 내부 클래스의 메서드 내부에서 생성하여 반환 할 수 있음.
- widget의 이벤트 핸들러에 활용됨

```java
class Outer{
    Runnable get Runnable(int i){
        int num = 100;

        // class MyRunnable implements Runnable{
        // 익명 내부 클래스 - 시작부터 리턴 때려버리기
        return new Runnable(){
            @Override
            public void run(){
                //num = 200 에러남
                // i = 10; 에러남
                System.out.println(i);
				System.out.println(num);
            }
        }; // 세미콜론 필수
    }

    // 아래처럼도 사용 가능...
    Runnable runner = new Runnable() {

		@Override
		public void run() {
			System.out.println("Runnable 이 구현된 익명 클래스 변수");

		}
	};

}
```

```java
// 사용
Outter2 out = new Outter2();

Runnable runnerble = out.getRunnable(10);
runnerble.run();

out.runner.run();

```
