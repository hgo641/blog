---
title: "Java - 람다식(Lambda expression)"
date: 2022-09-01
tags:
  - java
series: "java-programming"
---

## 스트림(Stream)

> 자료의 대상과 관계없이 동일한 연산을 수행한다. 다양한 데이터 소스를 통일한 방법으로 다루기 위한 방식이다.<br/>
>
> IntStream, Stream<Integer>, Stream<String> 

<br/>

* 배열, 컬렉션을 대상으로 연산을 수행한다.
* 스트림은 일회용이다. 한 번 연산을 수행하면 그 스트림은 소모되고 재사용할 수 없다.
* 스트림 연산은 기존 자료를 변경하지 않는다.
* 스트림 연산은 중간 연산과 최종 연산으로 구분된다.

### 📌 장점

일관성 있는 연산으로 자료의 처리를 쉽고 간단하게 할 수 있다. 아래 예시로 나올 코드들을 보면 람다 함수를 사용해 짧은 코드로 연산을 처리할 수 있다.



### 📌 중간 연산과 최종연산

중간연산은 조건에 맞는 요소를 추출하거나 반환한다. 최종연산이 호출될 때 중간 연산이 수행되고 결과가 생성된다.

* 중간연산 : filter(), map(), sorted() 등이 존재
* 최종연산 : forEach(), count(), sum() 등이 존재

```java
 sList.stream().filter(s->s.length() >= 5).forEach(s->System.out.println(s));

customerList.stream().map(c->c.getName()).forEach(s->System.out.println(s));
```

> filter(), map()이 중간연산이고 forEach()가 최종연산이다. (forEach는 스트림안의 요소들을 하나씩 가져온다. 매개변수인 s가 하나의 요소를 의미)



### 📌 스트림 사용 예시

```java
List<String> sList = new ArrayList<String>;
sList.add("Hongo");
sList.add("Gildong");
sList.add("AAA");

Stream<String> stream = sList.stream();
stream.forEach(s-> System.out.print("s\n"));
// Hongo
// Gildong
// AAA

stream.sorted().forEach(s-> System.out.print("s\n"));
// AAA
// Gildong
// Hongo

stream.map(s-> s.length()).forEach(s-> System.out.print("s\n"));
// 5
// 7
// 3

stream.filter(s->s.length()>=5).forEach(s-> System.out.print("s\n"));
// Hongo
// Gildong
```

* 배열과 콜렉션 객체에는 stream() 메서드가 있다. 해당 메서드를 사용해 Stream 객체를 생성할 수 있다.
* stream메서드안에는 람다식을 넣어 활용할 수 있다.



## reduce()

스트림에 정의된 연산이 아닌 프로그래머가 직접 구현한 연산을 적용할 수 있다.

```java
T reduce(T identify, BinaryOperator<T> accumulator)
```



### 📌 예시 - sum

```java
Arrays.stream(arr).reduce(0, (a,b)->a+b));
```

> 0은 sum의 초기값이고, 인자는 두 개가 들어온다.<br/>
>
> 스트림안의 모든 요소들을 돌며 계산을 한다.



### 📌 예시 - 여행 프로그램

* 여행 고객 Customer의 정보로는 name, age, price가 있다.
* age가 20미만인 고객의 price는 50이고 그렇지않은 고객의 price는 100이라고 해보자.
* 예시로 세 명의 고객을 생성한 뒤 스트림을 사용해 아래 기능을 구현해보자.

> 1. 전체 고객의 명단을 출력
> 2. 고객들의 price합을 출력
> 3. 20세 이상인 고객들의 명단 출력

```java
public class CustomerTest {

    public static void main(String[] args) {
        // 고객 세 명 생성 생성자는 (name, age, price)
        Customer c1 = new Customer("홍길동", 30, 100);
        Customer c2 = new Customer("홍고", 24, 100);
        Customer c3 = new Customer("신짱구", 7, 50);
		// 고객들을 담은 리스트 생성
        List<Customer> customerList = new ArrayList<Customer>();
        customerList.add(c1);
        customerList.add(c2);
        customerList.add(c3);

		
        System.out.println("고객명단");
        customerList.stream().map(c->c.getName()).forEach(c->System.out.println(c));

        System.out.println("\n전체 금액");
        int p = customerList.stream().map(c->c.getPrice()).reduce(0, (a,b)->a+b);
        // int p = customerList.stream().mapToInt(c->c.getPrice()).sum();
        System.out.println(p);
    
        System.out.println("\n20세 이상 고객명단");
        customerList.stream().filter(c->c.getAge()>=20).map(c->c.getName()).sorted().forEach(s-> System.out.println(s));

    }

}
```

> 마지막 20세이상 고객명단 출력하는 부분에서 <br/>
>
> ```java
> customerList.stream().filter(c->c.getAge()>=20).sorted().forEach(s-> System.out.println(s));
> ```
>
> 이렇게 구현해서 에러났었다. Customer객체를 sorted() 할 수 없기때문이었다. map을 사용해 sort할 수 있는 객체로 바꿔주자.



### 📌 BinaryOperator

위의 예시처럼 reduce안에 람다식을 직접 구현하는 방식도 있지만 람다식이 긴 경우 인터페이스 `BinaryOperator` 를 구현한 클래스를 사용하는 방식도 있다.<br/>

apply()메서드를 오버라이딩 해서 안에 적용하고자 하는 연산을 작성하면 된다.<br/>

자세한건 아래 예시를 보면서 확인해보자.



### 📌 예시 - 길이가 가장 긴 문자열 찾기

```java
class CompareString implements BinaryOperator<String> {
    
    @Override
    public String apply(String s1, String s2){
        if (s1.getBytes().length() >= s2.getBytes().length()){
            return s1;
        }
        else{
            return s2;
        }
    }
}

public class ReduceTest {

	public static void main(String[] args) {

		String[] greetings = {"1", "22", "333", "4444", "55555"};
		
		System.out.println(Arrays.stream(greetings).reduce("", (s1, s2)-> 
		                          {if (s1.getBytes().length >= s2.getBytes().length) 
				                                  return s1;
		                          else return s2;})); 
		
		String str = Arrays.stream(greetings).reduce(new CompareString()).get(); //BinaryOperator를 구현한 클래스 이용
		System.out.println(str);
		                          
	}
}

```



### 

