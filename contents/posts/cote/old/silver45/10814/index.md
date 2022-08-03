---
title: "백준 10814 나이순 정렬"
date: 2022-08-02
tags:
  - cote
---

## 문제

[백준 10814 나이순 정렬](https://www.acmicpc.net/problem/10814)
<br/>

## 풀이

나이는 1부터 200까지만 주어지므로 vector배열을 생성하면 쉽게 풀 수 있다.

<br/>>

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<string> v[201];
int n, age;
string name;
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> age >> name;
		v[age].push_back(name);
	}
	for (int i = 1; i < 201; i++) {
		for (auto s : v[i]) {
			cout << i <<" "<< s << "\n";
		}
	}
}
```
