---
title: "백준 2979 트럭주차"
date: 2022-06-25
tags:
  - cote
---

## 문제

상근이는 트럭을 총 세 대 가지고 있다. 오늘은 트럭을 주차하는데 비용이 얼마나 필요한지 알아보려고 한다. <br/>

상근이가 이용하는 주차장은 주차하는 트럭의 수에 따라서 주차 요금을 할인해 준다.<br/>

트럭을 한 대 주차할 때는 1분에 한 대당 A원을 내야 한다. 두 대를 주차할 때는 1분에 한 대당 B원, 세 대를 주차할 때는 1분에 한 대당 C원을 내야 한다.<br/>

A, B, C가 주어지고, 상근이의 트럭이 주차장에 주차된 시간이 주어졌을 때, 주차 요금으로 얼마를 내야 하는지 구하는 프로그램을 작성하시오.

<br/>

## 풀이

세 대의 트럭이 타임테이블별로 주차하는 것을 표현하면 다음과 같다.

| 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1   | 1   | 1   | 1   | 1   |     |     |     |
|     |     | 1   | 1   |     |     |     |     |
|     | 1   | 1   | 1   | 1   | 1   | 1   |     |

A, B, C가 각각 5, 3, 1이라면 총 주차비용은 <br/>

5 + 3\*2 + 1\*3 + 1\*3 + 3\*2 + 5 + 5 = 33이 된다. <br/>

이를 가지고 코드를 짜면 다음과 같다.

```cpp

#include<iostream>
#include<tuple>
#include<vector>

using namespace std;

int arr[105] = { 0, };
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	int a, b, c;
	cin >> a >> b >> c;
	int s, d;
	for (int i = 0; i < 3; i++) {
		cin >> s >> d;
		for (int j = s; j < d; j++) {
			arr[j] = arr[j] + 1;
		}
	}
	int sum = 0;
	for (int i = 0; i < 105; i++) {
		if (arr[i] > 0) {
			if (arr[i] == 1) {
				sum = sum + a;
			}
			else if (arr[i] == 2) {
				sum = sum + b*2;
			}
			else {
				sum = sum + c*3;
			}
		}


	}
	cout << sum;
}
```
