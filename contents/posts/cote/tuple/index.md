---
title: "c++로 tuple 만들기"
date: 2022-06-24
tags:
  - C++
  - cote
series: "코테준비"
---



## tuple?

tuple은 두 개 이상의 값을 순서대로 담을 때 사용됩니다.

> 보통 두 개만 담을 땐 pair를 사용하고
>
> 세 개 이상을 담을 땐 tuple을 사용!



## tuple 선언

```c++
tuple<int, char, string> test;
```

위 처럼 꺽쇠안에 원하는 자료형을 선언하면 된다.<br/>

차례대로 <int, char, string>이라는 원소를 가진 test라는 tuple이 생성된다.



## tuple 생성

tuple을 생성했으니 원하는 값을 tuple에 넣어보자!

```c++
test = make_tuple(1,'a',"hi");
```

test는 위 `tuple 선언`에서 선언한 tuple이다.<br/>

`make_tuple`이라는 함수를 사용해서 tuple에 값을 할당할 수 있다.



## tuple 값 가져오기

튜플에 할당한 값을 가져와보자

```c++
get<0>(test); // 1
get<1>(test); // 'a'
get<2>(test); // "hi"
```

`get`을 사용해 튜플의 값을 불러올 수 있다.<br/>

꺽쇠안에는 불러오고 싶은 값의 `index`를 넣고 괄호안에는 어떤 튜플에서 값을 불러올 것 인지를 명시한다.



## tuple끼리 값을 서로 바꾸기

```c++
// 형태가 똑같은 두 튜플 tl과 test가 있다고 할 때
swap(tl, test);
```

`swap`을 사용해 두 튜플의 값을 서로 바꿀 수 있다.



## tuple의 값을 다른 변수에 할당하기

```c++
int myint;
char mychar;
string mystr;

tie(myint, mychar, mystr) = test;
```

`tie`를 사용해 변수에 튜플의 값을 할당한다.<br/>

test는 <int, char, string> 형태이므로 tie 괄호안의 변수에 차례대로 할당된다.



## tupe 사용 예시

```c++
#include<bits/stdc++.h>

using namespace std;

int main(){
	tuple<int, int, int> tl;
	int a, b, c;
	char d;
	
	pi = {1, 2};
	tl = make_tuple(1,2,3);
	tie(a,b) = pi;
	cout << a <<", " <<b<<"\n";
	
	tie(a,b,c) = tl;
	cout << a <<", " <<b<< ", "<< c <<"\n";
	
    // tuple 선언
	tuple<int, int, char> tl;
    tuple<int, int, char> tl2;
    
    // tuple 생성 : make_tuple
	tl = make_tuple(1,2,'a');
    tl2 = make_tuple(3,4,'b');
    
    // tuple 값 가져오기 : get
    cout << get<2>(tl) << "\n";
    cout << get<2>(tl2) << "\n";
    
    // tuple 값 서로 바꾸기
    swap(tl,tl2);
    
    // tuple 값 다른 변수에 저장
	int a, b;
    char c;
    tie(a,b,c) = tl;
}
```

