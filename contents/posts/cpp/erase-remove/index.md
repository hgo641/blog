---
title: "[c++] erase()와 remove()를 사용해 특정 원소 삭제하기"
date: 2022-09-15
tags:
  - cote
  - cpp
---

## erase()

vector에는 erase()라는 내장 함수가 있는데, 위치를 인자로 넣어주면 해당 위치에 들어가는 원소들을 삭제한다.



### 간단 예시

`vector<string> v = {"a", "b", "c", "d"}`가 있다고 해보자.

```cpp
v.erase(v.begin()) // v의 맨 앞인 "a"가 삭제된다.
```

```cpp
v.erase(v.begin()+2) // v[2]인 "c"가 삭제된다.
```

```cpp
v.erase(v.end()-1) // 마지막 원소인 "d"가 삭제된다.
```

```cpp
v.erase(v.begin(), v.end()) // 전부 삭제
```

```cpp
v.erase(v.begin()+2, v.end()-1); // "c"가 삭제된다.
// v = {"a", "b", "d"}
```

인자를 두 개 넣을 수도 있는데, v.erase(a, b)일 때 v안의 a위치에서 b전까지 전부 삭제하겠다는 의미이다.



## remove()

`algorithm`헤더에 있는 함수로, 시퀀스 컨테이너에서 사용할 수 있다. (표준 연관 컨테이너(set, multiset, map)은 erase를 사용) <br/>

* `remove(탐색 시작 위치, 탐색 종료 위치, 삭제할 원소)`

세 번째 인자로 지워야할 원소값을 받고 해당 원소를 찾아 지워버린다는 것은 erase와 동일하나, 완전히 vector안에서 해당 원소를 삭제해 벡터의 size를 줄여버리는 erase와 달리 remove()는 원소를 완전히 삭제하진않고 지워야 할 원소를 벡터의 뒤로 이동시킨다.

```cpp
vector<string> v = {"a", "b", "c", "b" "d"}; 
v.remove(v.begin(), v.end(), "b");

for(string s : v){
    cout << s;
}

// v 내부 원소들
// a
// c
// d
// b
// ""
```

위처럼 타겟이 아닌 원소들을 앞으로 배치하고 타겟인 원소들은 뒤에 배치된다. (그 중 몇은 빈 값 ("")으로 바뀌기도 함)



## erase()와 remove()를 함께 사용하기

remove()는 반환값으로 지워야할 원소들의 시작 주소를 반환한다. erase()는 위치(주소)를 인자로 받아 삭제할 수 있으므로, remove()로 지우고 싶은 원소들을 뒤로 걸러낸 뒤, erase()를 사용해 해당 원소들을 완전히 삭제할 수 있다.



### 예시 - 문자열에서 공백 제거하기

```cpp
string str = " a b c d ";
str.erase(remove(str.begin(), str.end(), ' '), str.end());

// 출력 결과
// abcd
```

* str의 앞 뒤와 사이에는 공백이 존재한다.
* remove로 ' '를 찾아 str의 뒤에 배치한 뒤,
* erase를 사용해 ' '를 완전히 삭제할 수 있다.
