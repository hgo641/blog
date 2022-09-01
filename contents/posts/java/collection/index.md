---
title: "Java - Collection 프레임워크"
date: 2022-09-01
tags:
  - java
series: "java-programming"
---

## Collection 프레임워크

* 프로그램 구현에 필요한 자료구조(Data Structure)를 구현해 놓은 JDK 라이브러리
* java.util 패키지에 구현되어 있음
* 개발에 소요되는 시간을 절약하면서 최적화 된 알고리즘을 사용할 수 있음

![](collection.png)



## 📌 Collection 인터페이스

* 하나의 객체를 관리하기 위한 메서드가 선언된 인터페이스
  * [String, String, String ,...]

* 하위에 List와 Set 인터페이스가 있음



## 📌 List 인터페이스

* 객체를 순서에 따라 저장하고 관리하는데 필요한 메서드가 선언된 인터페이스
* 자료구조 리스트 (배열, 연결리스트)의 구현을 위한 인터페이스
* 중복을 허용함
* ArrayList, Vector, LinkedList, Stack, Queue 등...

``` java
ArrayList<Member> arrayList = new ArrayList<Member>();

arrayList.add(member);
arrayList.remove(idx);
arrayList.get(idx);
arrayList.size();

```



## 📌 Map 인터페이스

* 쌍(pair)로 이루어진 객체를 관리하는데 사용하는 메서드들이 선언된 인터페이스
* 객체는 key-value의 쌍으로 이루어짐
* key는 중복을 허용하지 않음
* HashTable, HashMap, Properties, TreeMap 등이 Map 인터페이스를 구현 함



## 📌 Iterator

> Collection 요소를 순회한다.

<br/>

### 요소의 순회란?

* 컬렉션 프레임워크에 저장된 요소들을 하나씩 차례로 참조하는것
* 순서가 있는 List인터페이스의 경우는 Iterator를 사용 하지 않고 get(i) 메서드를 활용할 수 있음
* Set 인터페이스의 경우 get(i) 메서드가 제공되지 않으므로 Iterator를 활용하여 객체를 순회함



### Iterator 사용하기

* boolean hasNext() : 이후에 요소가 더 있는지를 체크하는 메서드, 요소가 있다면 true를 반환
* E next() : 다음에 있는 요소를 반환

```java
// 예시
public boolean removeMember(int memberId){  // 멤버 아이디를 매개변수로, 삭제 여부를 반환
    Iterator<Member> ir = arrayList.iterator();
    while(ir.hasNext()) { // 다음 요소가 없을 때까지 반복
        Member member = ir.next();
        int tempId = member.getMemberId();
        if(tempId == memberId){            // 멤버아이디가 매개변수와 일치하면 
            arrayList.remove(member);           // 해당 멤버를 삭제
            return true;                   // true 반환
        }
    }
    return false;		
}
```





## 📌 HashSet 클래스

* Set 인터페이스를 구현한 클래스이다.
* 멤버의 중복 여부를 체크하기 위해 인스턴스의 동일성을 확인해야 함 (멤버아이디가 같으면 논리적으로 같은 멤버)
* **필요한 경우, 동일성 구현을 위해 필요에 따라 equals()와 hashCode()메서드를 재정의해줘야함**
  * 재정의가 되어있지않다면 new Member를 한 member인스턴스 두 개인 m1과 m2의 멤버넘버가 둘 다 100일 경우, hashset에 m1과 m2 둘 다 들어감.
  * 멤버넘버가 같은면 논리적으로 같다고 재정의를 해줄 경우, m1과 m2는 동일한 인스턴스로 판단되어 중복되지않고 하나만 들어감.

<br/>

```java
// Member.java

...

    @Override
	public int hashCode() {
		return memberId;
	}

	@Override
	public boolean equals(Object obj) {
		if( obj instanceof Member){
			Member member = (Member)obj;
			if( this.memberId == member.memberId )
				return true;
			else 
				return false;
		}
		return false;
	}

...
```

<br/>

```java
// 예시

public class MemberHashSet {
	private HashSet<Member> hashSet;

	public MemberHashSet(){
		hashSet = new HashSet<Member>();
	}
	
	public void addMember(Member member){
		hashSet.add(member);
	}
	
	public boolean removeMember(int memberId){

		Iterator<Member> ir = hashSet.iterator(); // 요소들간 순서가 없으므로 Iterator 사용
		
		while( ir.hasNext()){
			Member member = ir.next();
			int tempId = member.getMemberId();
			if( tempId == memberId){
				hashSet.remove(member);
				return true;
			}
		}
		
		System.out.println(memberId + "가 존재하지 않습니다");
		return false;
	}
	
	public void showAllMember(){
		for(Member member : hashSet){
			System.out.println(member);
		}
		System.out.println();
	}
}

```





