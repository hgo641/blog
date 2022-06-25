---
title: "문자형을 숫자로 변환"
date: 2022-06-21
tags:
  - C++
  - cote
series: "코테준비"
---



## 문자를 숫자로 바꾸기

아스키코드를 이용하자!

> 'A' ~ 'Z' 는 아스키 코드로 65 ~ 90
>
> 'a' ~ 'z' 는 아스키 코드로 97 ~ 122



* 만약 'a' ~ 'z' 사이의 숫자를 입력 받아 숫자 0 ~ 26으로 표현하고 싶다면

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    char c;
    cin >> c;
    cout << (int)c - 97 <<"\n";
    return 0;
}
```

<br/>

* 사실 이렇게 해도 됨

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    char c;
    cin >> c;
    cout << (int)c - 'a' <<"\n";
    return 0;
}
```



