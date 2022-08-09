---
title: "백준 2805 나무 자르기"
date: 2022-08-09
tags:
  - cote
---

## 문제

[백준 2805 나무 자르기](https://www.acmicpc.net/problem/2805)



## 첫 번째 풀이

```cpp
#include <bits/stdc++.h>
using namespace std;

long long  sum, cur, mid, high, low, n, m, k;
vector<int> v;
int cut_tree() {
	while (high >= low) {
		sum = 0;
		mid = (high + low) / 2;
		for (int i = 0; i < mid; i++) {
			sum += v[i] - v[mid];
		}
		if (sum < m) {
			low = mid + 1;
		}
		else if (sum > m) {
			high = mid - 1;
		}
		else if (sum == m) {
			cout << v[mid];
			return 1;
		}
	}
	// 상세 길이 조정
	int _mid = mid;
	if (sum > m) {
		high = v[mid-1];
		low = v[mid];
		
		while (high >= low) {
			mid = (high + low) / 2;
			sum = 0;
			for (int i = 0; i < _mid; i++) {
				sum += v[i] - mid;
			}
			if (sum == m) {
				cout << mid;
				return 1;
			}
			else if (sum > m) {
				low = mid + 1;
			}
			else if (sum < m) {
				high = mid - 1;
			}
		}
		if (sum < m) {
			cout << mid - 1;
		}
		else {
			cout << mid;
		}
		return 1;
	}
	else if (sum < m) {
		high = v[mid];
		low = v[mid+1];
		while (high >= low) {
			mid = (high + low) / 2;
			sum = 0;
			for (int i = 0; i <= _mid; i++) {
				sum += v[i] - mid;
			}
			if (sum == m) {
				cout << mid;
				return 1;
			}
			else if (sum > m) {
				low = mid + 1;
			}
			else if (sum < m) {
				high = mid - 1;
			}
		}
		if (sum < m) {
			cout << mid - 1;
		}
		else {
			cout << mid;
		}
		return 1;
	}

	
}
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n >> m;
	v.push_back(0);
	for (int i = 0; i < n; i++) {
		cin >> k;
		v.push_back(k);
	}
	sort(v.begin(), v.end(), greater<>());

	high = v.size() - 1;
	low = 0;
	cut_tree();

}
```

