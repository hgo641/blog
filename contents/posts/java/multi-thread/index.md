---
title: "Java -  멀티 Thread 프로그래밍에서의 동기화"
date: 2022-09-07
tags:
  - java
series: "java-programming"
---

## critical section과 semaphore

- critical section  은 두 개 이상의 thread가 동시에 접근 하는 경우 문제가 생길 수 있기 때문에 동시에 접근할 수 없는 영역
- semaphore 는 특별한 형태의 시스템 객체이며 get/release 두 개의 기능이 있다.
- 한 순간 오직 하나의 thread 만이 semaphore를 얻을 수 있고, 나머지 thread들은 대기(blocking) 상태가 된다.
- semaphore를 얻은 thread 만이 critical section에 들어갈 수 있다.

[[critical-section]](https://blog.hongo.app/8-1/)<br/>

[[Synchronization Tool]](https://blog.hongo.app/sync-tools/)



### 📌 예제 - 은행 입금&출금

```java
class Bank{
	private int money = 10000;
	
	public int getMoney() {
		return money;
	}

	public void setMoney(int money) {
		this.money = money;
	}

	public void saveMoney(int save) throws InterruptedException {
        int m = getMoney();
		Thread.sleep(3000);
		setMoney(m+save);
	}
	public void minusMoney(int minus) throws InterruptedException {
        int m = getMoney();
		Thread.sleep(100);
		setMoney(m-minus);
	}
}

class Worker extends Thread{
	public void run() {
		System.out.println("일한 만큼 저축했다.");
		try {
			BankTest.bank .saveMoney(3000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("저축 후 금액 : "+BankTest.bank.getMoney());
		
	}
}

class Shopper extends Thread{
	public void run() {
		System.out.println("쇼핑할 돈을 뽑는다.");
		try {
			BankTest.bank.minusMoney(1000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("쇼핑 후 금액 : "+BankTest.bank.getMoney());
		
	}
}
public class BankTest {

	public static Bank bank = new Bank();

	public static void main(String[] args) throws InterruptedException {
		// TODO Auto-generated method stub
		Worker worker = new Worker();
		Shopper shopper = new Shopper();
		
		worker.start();
		shopper.start();
		
		worker.join();
		shopper.join();
		
		Thread.sleep(3000);
		System.out.println("최종 금액 : "+bank.getMoney());
	}

}
```

* 위와 같이 Bank에 돈을 저축하는 Worker와 돈을 뽑아오는 Shopper가 있다.
* 기존 금액 10000원에서 Worker가 3000원을 입금하고 Shopper가 1000원을 출금하므로 남은 돈은 12000원이어야한다.
* 그러나 출력결과는 아래와 같다.

```
일한 만큼 저축했다.
쇼핑할 돈을 뽑는다.
쇼핑 후 금액 : 9000
저축 후 금액 : 13000
최종 금액 : 13000
```

> 코드를 보면 입금을 하는데는 3초의 시간이 걸리지만 출금을 하는데는 0.1초의 시간이 걸린다. Worker가 입금을 수행하는 사이 출금 메서드가 수행&종료되었기에 최종 금액에는 마지막 Worker 메서드가 수행한 결과만 남았다.

<br/>

이처럼 공유 자원인 돈에 동시에 접근해서 각자의 메서드를 수행했기에 한 쪽의 수행결과는 적용되지않는 현상이 일어났다. 문제를 해결하기 위해서는 공유 자원에 동시 접근이 불가능하게 코드를 짜야한다.



### 📌 synchronized 

동기화하기 위해서는 `synchronized`키워드를 사용하면 된다. 두 가지 방식으로 사용할 수 있다.

<br/>

#### `1. synchronized method`

동기화가 필요한 함수 선언부에 synchronized 키워드를 붙인다. 해당 함수를 실행할 때는 함수가 포함된 객체의 자원들(this)에 lock을 건다. <br/>

ex) `public synchronized void saveMoney(){};`



#### `2. synchronized block`

```
synchronized (mutex){
	...
}
```

`synchronized` 키워드가 붙은 블록을 만든다. 괄호안에는 접근을 제한할 자원이 들어간다.

<br/>

### 적용

* 우리가 동시접근을 제어해야할 클래스는 Bank이다. Bank안에서 `synchronized`를 사용해보자

```java
class Bank{
	private int money = 10000;
	
	public int getMoney() {
		return money;
	}

	public void setMoney(int money) {
		this.money = money;
	}

    // 1. 동기화가 필요한 함수의 선언부에 synchronized를 붙인다.
	public synchronized void saveMoney(int save) throws InterruptedException {
        int m = getMoney();
		Thread.sleep(3000);
		setMoney(m+save);
	}
	public void minusMoney(int minus) throws InterruptedException {
        synchronized(this){
            int m = getMoney();
            Thread.sleep(100);
            setMoney(m-minus);
        } 
	}
}


// 물론 thread에 적용하는 것도 가능
public void run(){
    synchronized (BankTest.bank){
        ...
    }
}
```



```
// 실행 결과
일한 만큼 저축했다.
저축 후 금액 : 13000
쇼핑할 돈을 뽑는다.
쇼핑 후 금액 : 12000
최종 금액 : 12000
```



## wait()/notify() 메서드를 활용한 동기화 프로그래밍

* 리소스가 어떤 조건에서 더 이상 유효하지 않은 경우 리소스를 기다리기 위해 Thread 가 wait() 상태가 된다.
* wait() 상태가 된 Thread은 notify() 가 호출 될 때까지 기다린다.
* 유효한 자원이 생기면 notify()가 호출되고 wait() 하고 있는 Thread 중 무작위로 하나의 Thread를 재시작 하도록 한다.
* notifyAll()이 호출되는 경우 wait() 하고 있는 모든 Thread가 재시작 된다.
* 이 경우 유효한 리소스만큼의 Thread만이 수행될 수 있고 자원을 갖지 못한 Thread의 경우는 다시 wait() 상태로 만든다.
* **자바에서는 notifyAll() 메서드의 사용을 권장한다.**



### 📌 예제 - 도서관 책 대여

* Do it이라는 책을 세 개 가진 Library가 있다고 하자
* Student는 Library에서 책을 빌려갈 수 있다. 이 때 책은 가장 앞에 있는 책을 가져간다.
* Student는 빌린 책을 다시 반납할 수 있다.

```java
class Library{
	ArrayList<String> books = new ArrayList();
	Library(){
		books.add("Do it 1");
		books.add("Do it 2");
		books.add("Do it 3");

	}
	
	public synchronized String rendBook() throws InterruptedException {
		Thread t = Thread.currentThread();
		if (books.isEmpty()) {
			System.out.println(t.getName()+": 책을 빌리지못함.");
		} 
	
		String book = books.remove(0);
		System.out.println(t.getName()+": " +book+"을 빌림");
		return book;
	}
	
	public synchronized void returnBook(String book) {
		Thread t = Thread.currentThread();
		books.add(book);
		System.out.println(t.getName()+": " +book+"을 반납함");
	}
}

class Student extends Thread{
	public void run() {
		try {
			String book = LibraryMain.lib.rendBook();
			sleep(5000);
			LibraryMain.lib.returnBook(book);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
	}
}

public class LibraryMain {

	static Library lib = new Library();
	public static void main(String[] args) {
		Student s1 = new Student();
		Student s2 = new Student();
		Student s3 = new Student();
		
		s1.start();
		s2.start();
		s3.start();
	}

}
```

```
Thread-0: Do it 1을 빌림
Thread-2: Do it 2을 빌림
Thread-1: Do it 3을 빌림
Thread-1: Do it 3을 반납함
Thread-0: Do it 1을 반납함
Thread-2: Do it 2을 반납함
```

* 책의 수만큼 Student가 있을 경우 모든 학생이 책을 빌릴 수 있다. 그럼 책의 수보다 Student가 더 많을 때는 어떻게 될까?

<br/>

```
Thread-1: Do it 1을 빌림
Thread-0: Do it 2을 빌림
Thread-2: Do it 3을 빌림
Thread-3: 책을 빌리지못함.
Thread-4: 책을 빌리지못함.
Thread-5: 책을 빌리지못함.
Thread-0: Do it 2을 반납함
Thread-2: Do it 3을 반납함
Thread-1: Do it 1을 반납함
```

* 당연히 책을 빌리지 못하는 학생이 생긴다.
* 누군가 책을 반납하면 아직 책을 빌리지 못한 학생이 책을 빌릴 수 있게 동기화를 시켜보자.



### 📌 notify()를 사용하는 경우

```java
public synchronized String rendBook() throws InterruptedException {
		Thread t = Thread.currentThread();
    // 책이 없다면 wait() 
		if (books.isEmpty()) {
			System.out.println(t.getName()+": waiting...");
			wait();
			System.out.println(t.getName()+": End waiting!");
		} 
	
		String book = books.remove(0);
		System.out.println(t.getName()+": " +book+"을 빌림");
		return book;	
	}
```

```java
public synchronized void returnBook(String book) {
		Thread t = Thread.currentThread();
		books.add(book);
		System.out.println(t.getName()+": " +book+"을 반납함");
    // wait중인 스레드 중 하나를 임의로 깨움
		notify();
	}
```



```
Thread-0: Do it 1을 빌림
Thread-5: Do it 2을 빌림
Thread-1: Do it 3을 빌림
Thread-2: waiting...
Thread-3: waiting...
Thread-4: waiting...
Thread-1: Do it 3을 반납함
Thread-2: End waiting!
Thread-2: Do it 3을 빌림
Thread-0: Do it 1을 반납함
Thread-5: Do it 2을 반납함
Thread-4: End waiting!
Thread-4: Do it 1을 빌림
Thread-3: End waiting!
Thread-3: Do it 2을 빌림
Thread-4: Do it 1을 반납함
Thread-2: Do it 3을 반납함
Thread-3: Do it 2을 반납함
```

* notify()로 wait()중인 스레드 중 하나를 임의로 깨워 모든 학생들이 책을 빌리게 할 수 있다.



### 📌 notifyAll()을 사용하는 경우

앞의 코드에서 notify를 notifyAll로 바꾸면 어떻게 될까?

```
Thread-0: Do it 1을 빌림
Thread-2: Do it 2을 빌림
Thread-3: Do it 3을 빌림
Thread-4: waiting...
Thread-5: waiting...
Thread-1: waiting...
Thread-3: Do it 3을 반납함
Thread-4: End waiting!
Thread-4: Do it 3을 빌림
Thread-1: End waiting!
Exception in thread "Thread-1" Thread-5: End waiting!
Thread-2: Do it 2을 반납함
Exception in thread "Thread-5" java.lang.IndexOutOfBoundsException: Index 0 out of bounds for length 0

```

* notifyAll은 wait중인 모든 스레드를 깨운다.
* wait중인 모든 스레드가 wait을 마치고 book을 가져가려고 한다.
* 먼저 자원을 가져간 스레드가 있다면 나중에 온 스레드는 이미 없는 자원을 가져가려고 하는게 된다.
* 위의 경우 Thread-4가 제일 먼저 책을 가져가고 다시 책장은 비게 되었다.
* 그러나 아직 책을 빌리지 못한 Thread-1과 Thread-5도 wait에서 깨어났기 때문에 책을 빌리려한다.
* 빈 책장에서 책을 가져가려고 했기에 에러가 난다.

<br/>

#### 그럼 어떻게 해결할 수 있을까?

```java
public synchronized String rendBook() throws InterruptedException {
		Thread t = Thread.currentThread();
    // while문으로 변경
		while (books.isEmpty()) {
			System.out.println(t.getName()+": waiting...");
			wait();
			System.out.println(t.getName()+": End waiting!");
		} 
	
		String book = books.remove(0);
		System.out.println(t.getName()+": " +book+"을 빌림");
		return book;	
	}
```

* if문을 while문으로 바꾸면된다.
* 해당 메서드는 `synchronized`이기 때문에 여러 스레드가  동시에 wait에서 풀려나도 자원에는 하나의 스레드만 접근하게 된다.
* 그렇기에 먼저 자원에 접근한 스레드가 book을 빌려서 책장이 비게 된다면,
* wait에서 벗어난 스레드들은 while문의 조건이 true가 되어 다시 wait()을 하게 된다.

```
// 출력 결과
Thread-0: Do it 1을 빌림
Thread-5: Do it 2을 빌림
Thread-4: Do it 3을 빌림
Thread-3: waiting...
Thread-2: waiting...
Thread-1: waiting...
Thread-5: Do it 2을 반납함
Thread-3: End waiting!
Thread-3: Do it 2을 빌림
Thread-1: End waiting!
Thread-1: waiting...
Thread-2: End waiting!
Thread-2: waiting...
Thread-4: Do it 3을 반납함
Thread-0: Do it 1을 반납함
Thread-2: End waiting!
Thread-2: Do it 3을 빌림
Thread-1: End waiting!
Thread-1: Do it 1을 빌림
Thread-3: Do it 2을 반납함
Thread-1: Do it 1을 반납함
Thread-2: Do it 3을 반납함
```

