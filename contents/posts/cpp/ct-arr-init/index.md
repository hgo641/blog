---
title: "fill과 memset을 이용해 배열 초기화"
date: 2022-06-25
tags:
  - C++
  - cote
series: "코테준비"
---

## 배열 초기화





## 배열 초기화

fill과 memset을 사용해 배열을 초기화해보자!



### fill (시작주소, 끝주소, 값)

fill은 시작주소부터 끝주소까지를 주어진 값으로 초기화한다.

아래처럼 사용할 수 있다.

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
	int arr[5];
	fill(arr, arr+5, 1);
	for (int a : arr) cout << a << " ";
	cout << "\n";
    // output : 1 1 1 1 1
    
    char str[] = "hello";
	fill(str, str + 3, 'c');
	for (char c : str) cout << c << " ";
    // output : c c c l o
}
```



### memset(시작주소, 값, 사이즈)

memset은 시작주소부터 사이즈까지를 주어진 값으로 초기화한다.<br/>

배열의 모든 값을 초기화하려면 시작주소에 배열의 이름을 넣어도 무방하다. (배열의 이름은 배열의 첫 번째 주소를 가리킨다.) <br/>

사이즈는 바이트 단위로 `sizeof(배열)` 로 선언할 수 있다.

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
	int arr[5];
    // sizeof(arr)은 int형 5개가 있으므로 4byte * 5 = 20
	memset(arr, 0, sizeof(arr)); 
	for (int a : arr) cout << a << " ";
    // output : 0 0 0 0 0
    
    char str[] = "hello";
    // c++에서 char은 1byte
    memset(str, 'c', 3);
    for (char c : str) cout << c << " ";
    // output : c c c l o
}
```

<br/>

#### 주의 : memset으로 int배열을 0이 아닌 다른 수로 초기화할 수 있을까?

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
	int arr[5];
    // memset을 사용해 1로 초기화해보자!
	memset(arr, 1, sizeof(arr)); 
	for (int a : arr) cout << a << " ";
    // output : 16843009 16843009 16843009 16843009 16843009
}
```

위처럼 `memset` 은  0이 아닌 다른 수로 초기화할 수 없다.<br/>

`memset`은 1 byte단위로 값을 초기화한다. 때문에 1을 값으로 넣는다면  `memset`은 1byte단위로 1을 만들게된다.  int형은 4byte로 표현되어야 하므로 알 수 없는 값으로 초기화된다.

> 0은 1byte로 표현되어도 00000000이기 때문에 동일
>
> char도 1byte이므로 가능

**즉, memset은 0과 char로만 초기화가 가능하다!**



## 2차원배열 초기화

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    int arr[5][5];
    fill(&arr[0][0], &arr[0][0]+25, 1);
    for(int i=0; i<5; i++){
		for(int j = 0; j<5; j++) cout << arr[i][j]<<" ";
		cout << "\n";
	}
    memset(arr, 0, sizeof(arr));
    for(int i=0; i<5; i++){
		for(int j = 0; j<5; j++) cout << arr[i][j]<<" ";
		cout << "\n";
	}
}
```

