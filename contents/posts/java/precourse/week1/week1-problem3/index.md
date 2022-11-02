---
title: "우테코5기 프리코스 1주차 Problem3"
date: 2022-11-02
tags:
  - java
series: "wooteco-precourse"
---

## Problem3 : 369 게임

* 369 게임중, 어떤 숫자 number가 주어졌을 때, 1부터 number까지 쳐야하는 손뼉의 개수를 반환하는 문제였다.
* number는 1 이상 10,000 이하인 자연수이다.



### 📗 기능 목록

```
📌 `int getNumberOfClap(String number)`
* `number`에 들어가는 3, 6, 9의 개수를 반환

📌 `int getCountOfClap(int number)`
* 1부터 number까지 쳐야하는 손뼉의 개수를 반환
```



### 📌 코드 

[깃허브 코드](https://github.com/hgo641/java-onboarding/blob/hgo641/src/main/java/onboarding/Problem3.java)

* 굉장히 심플하게 풀었다. 1부터 number까지 모든 숫자들을 for문으로 순회하며, 각 숫자에 들어가는 3, 6, 9의 개수를 카운트해서 풀었다.



### ✏ 다른 분의 풀이

[mihye126](https://potatosprout.tistory.com/13)님의 허락을 받고 슬랙에 공유해주신 풀이를 가져왔다.

```java
private static List<String> parseDigits(int num) {
    List<String> digits = new ArrayList<>();

    for (int i = 1; i <= num; i++) {
        String s = Integer.toString(i);
        digits.addAll(Arrays.asList(s.split("")));
    }
    return digits;
}

public static int solution(int number) {
    List<String> target = Arrays.asList("3", "6", "9");
    List<String> digits = parseDigits(number);
    digits.retainAll(target);

    return digits.size();
}
```

3, 6, 9를 카운트 하는 방법에 차이가 있는데, 369가 담긴 콜렉션 객체를 만들고 1부터 number까지의 모든 수의 digit을 저장하는 리스트를 만들어 `retainAll`을 적용하는 풀이였다. `retainAll`을 사용해 교집합을 수행할 수 있음을 배웠다.<br/>

또, 자린이인 나는 Arrays.asList()또한 생소하였다. 인자로 들어간 것들로 List를 만든다는 것일까? 이에 관해 찾아본 것을 포스팅 [Java - new ArrayList<>() vs Arrays.asList() vs List.of()](https://blog.hongo.app/arrayList-vs-asList/)로 정리하였다.



