---
title: "[c++] stringstream을 사용해서 공백 기준으로 문자열 자르기"
date: 2022-09-15
tags:
  - cote
  - cpp
---

## 공백 기준으로 문자열 자르기

`"name hongo"`라는 문자열이 있을 때 공백을 기준으로 `"name"`과 `"hongo"`를 따로 저장하고 싶을 때가 있다.<br/>

`substr()`과 string의 내장 함수 `find()`를 사용하면 쉽게 분리할 수 있다.

```cpp
string str = "name hongo";

string w1 = str.substr(0, str.find(" "));
string w2 = str.substr(str.find(" ") + 1);
cout << w1 << "\n"; // name
cout << w2 << "\n"; // hongo
```

<br/>

그런데 만약 공백이 여러개라면 어떻게 해야할까?<br/>

`"name hongo age 24"` 라는 문자열이 있을 때, 공백을 기준으로 name, hongo, age, 24 이렇게 네 개의 문자열로 나누고 싶다면 여러 번의 substr()이 실행되어야 할 것이다.<br/>

다행히도 substr()외에 공백을 기준으로 문자열을 나눌 수 있는 편리한 방법이 있다.



## stringstream

> 문자열에서 원하는 자료형의 데이터를 뽑는데 사용한다.

* `<sstream>` 헤더가 필요하다.
* 공백과 "\n"을 기준으로 데이터를 구분한다.



```cpp
string str = "name hongo age 10";

stringstream ss(str);

string word;
while (ss >> word) {
    cout << word << "\n";
}

// 출력 결과 : word가 string이므로 공백과 "\n"을 기준으로 string을 뽑아낸다.
// name
// hongo
// age
// 10
```

```cpp
string str = "1 2 3 4";
stringstream ss(str);
int num;

while (ss >> num) {
    cout << num << "\n";
}

// 출력 결과 : 공백과 "\n"을 기준으로 int를 뽑아낸다.
// 1
// 2
// 3
// 4
```

```cpp
string str = "1 2a 3 4";
stringstream ss(str);
int num;

while (ss >> num) {
    cout << num << "\n";
}

// 출력 결과 : 공백과 "\n"을 기준으로 int를 뽑아낸다. int가 아닌 자료형이 등장하면 멈춘다. 
// 1
// 2

// 2뒤에 int가 될 수 없는 a가 나오므로 멈춘다.
```





