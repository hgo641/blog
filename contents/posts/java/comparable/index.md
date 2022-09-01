---
title: "Java - TreeSet과 Comparable&Comparator"
date: 2022-09-01
tags:
  - java
series: "java-programming"
---

## TreeSet 클래스

- 객체의 정렬에 사용하는 클래스

- Set 인터페이스를 구현하여 중복을 허용하지 않고, 오름차순이나 내림차순으로 객체를 정렬할 수 있음

- 내부적으로 이진검색트리(binary search tree)로 구현됨

- 이진검색트리에 저장하기 위해 각 객체를 비교해야 함

  > 루트 왼쪽 노드는 루트보다 값이 작고, 오른쪽 노드는 루트보다 값이 크다.

- 비교 대상이 되는 객체에 Comparable이나 Comparator 인터페이스를 구현 해야 TreeSet에 추가 될 수 있음
- String, Integer등 JDK의 많은 클래스들이 이미 Comparable을 구현했음

### 📌 예제1 - String

```java
import java.util.TreeSet;

public class TreeSetTest {

	public static void main(String[] args) {

		TreeSet<String> treeSet = new TreeSet<String>();
		treeSet.add("홍길동");
		treeSet.add("강감찬");
		treeSet.add("이순신");

		for(String str : treeSet) {
			System.out.println(str);
            // 강감찬
            // 이순신
            // 홍길동
		}
	}
}

```

> String 클래스는 이미 Comparable 인터페이스가 구현되어 있으므로 오름차순으로 정렬되어 출력된다.

### 📌 예제2 - Comparable

```
public class MemberTreeSet {

	private TreeSet<Member> treeSet;
	...
}
```

- Member TreeSet을 지닌 MemberTreeSet 클래스가 있다.

<br/>

```java
public class MemberTreeSetTest {

	public static void main(String[] args) {

		MemberTreeSet memberTreeSet = new MemberTreeSet();

		Member memberKim = new Member(1003, "김유신");
		Member memberLee = new Member(1001, "이순신");
		Member memberKang = new Member(1002, "강감찬");

		memberTreeSet.addMember(memberKim);
		memberTreeSet.addMember(memberLee);
		memberTreeSet.addMember(memberKang);
		memberTreeSet.showAllMember();

	}
}

```

- 실행하면 Comparable이 없으므로 에러가 난다.
- Member클래스 아이디 오름차순으로 정렬되게 Comparable 인터페이스를 구현해보자!

<br/>

```java
public class Member implements Comparable<Member>{

	......

	@Override
	public int compareTo(Member member) {
		//return (this.memberId - member.memberId);   //오름차순
		return (this.memberId - member.memberId) *  (-1);   //내림 차순
	}
}

```

> - this가 새로 들어오는 멤버이고,
> - member가 기존에 있었던 member라고 생각하면 편하다.
>
> 양수가 반환되면 순서가 바뀌지않고, 음수가 반환되면 this와 member의 순서가 바뀐다.

### 📌 예제3 - Comparator

- 이미 Comparable이 구현된 경우 Comparator로 비교하는 방식을 다시 구현할 수 있다.

String은 디폴트로 오름차순 Comparable이 구현되어있다. String의 비교함수를 Comparator로 다시 구현해보자.

```java
class MyCompare implements Comparator<String>{

	@Override
	public int compare(String s1, String s2) {
		return (s1.compareTo(s2)) *-1 ; // 기존 comparator의 반대
	}
}
```

```java
public class ComparatorTest {

	public static void main(String[] args) {

		Set<String> set = new TreeSet<String>(new MyCompare());
		set.add("aaa");
		set.add("ccc");
		set.add("bbb");

		System.out.println(set);
        // ccc
        // bbb
        // aaa
	}
}
```

> Comparator를 적용할 때는 treeset을 선언할 때 인자로 Comparator가 선언되어있는 클래스를 넣어줘야한다.
