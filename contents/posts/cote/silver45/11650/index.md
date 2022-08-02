---
title: "백준 11650 좌표정렬"
date: 2022-08-02
tags:
  - cote
---

## 문제

[백준 11650 좌표정렬](https://www.acmicpc.net/problem/11650)
<br/>

## 풀이

map을 만들고 map의 key와 value를 가지고 정렬을 하면 되겠지? 했지만, map의 value를 가지고 정렬을 하는건 불가능하고 vector<pair<int,int>>형식으로 map을 정렬한 것처럼 사용할 수 있었다. compare함수를 만들고 이를 벡터에 적용해 정렬했다.

<br/>

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, x, y;

bool compare(pair<int,int> _Left, pair<int,int> _Right) {
	if (_Left.first == _Right.first) return _Left.second < _Right.second;
	else return _Left.first < _Right.first;
}

int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	vector <pair<int, int>> v;
	for (int i = 0; i < n; i++) {
		cin >> x >> y;
		v.push_back({ x,y });
	}
	sort(v.begin(), v.end(), compare);

	for (auto a : v) {
		cout << a.first << " " << a.second << "\n";
	}
}
```
