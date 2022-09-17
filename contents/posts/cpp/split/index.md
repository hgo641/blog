---
title: "[c++] 특정 문자를 기준으로 문자열 자르기"
date: 2022-09-18
tags:
  - cote
  - cpp
---

## istringstream

`stringstream`을 사용하면 공백과 "\n"을 기준으로 문자열을 자를 수 있지만, `istringstream`을 사용하면 다른 문자를 가지고도 편리하게 문자열을 자를 수 있다. <br/>
[stringstream 포스팅](https://blog.hongo.app/sstream/)
<br/>
<br/>
`istringstream`을 사용해서 ','를 기준으로 문자열을 잘라보자.

```cpp
string str = "hello,cpp,world!";
istringstream ss(str);
string temp;
while (getline(ss, temp, ',')) {
    cout << temp << "\n";
}
```

```cpp
// 출력 결과
hello
cpp
world!
```

## substr()과 find()를 함께 사용

- `find()`로 문자열을 자르는 기준이 되는 문자를 찾는다.
- `substr()`로 `find()`가 반환하는 인덱스를 사용해 문자열을 자른다.
- `find()`의 반환값이 string::npos 가 될 때까지 계속 탐색하면서 `substr()`을 반복한다.

```cpp
string str = "hello,cpp,world!";
string temp;

int idx = 0;
size_t find = str.find(',');
while (find != string::npos) {
    temp = str.substr(idx, find - idx);
    idx = find + 1;
    cout << temp << "\n";
    find = str.find(',', idx); // idx부터 탐색
}
temp = str.substr(idx);
cout << temp << "\n";
```
