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

처음에는 이진 탐색으로 탐색하지않고 처음부터 하나 하나 탐색했더니 시간초과가 나왔다...<br/>

> 순차적인 탐색은 최대 O(n)이 걸리고 이진 탐색은 O(logn)만에 할 수 있다. <br/>
>
> 이 문제의 시간제한은 1초인데, 1억 정도의 연산이면 1초가 걸린다고한다.<br/>
>
> * 1 ≤ N ≤ 1,000,000  - 나무의 수
> * 1 ≤ M ≤ 2,000,000,000 - 필요한 나무의 총 길이
> * 0 ≤ h ≤ 1,000,000,000 - 각 나무들의 길이
>
> 순차적으로 탐색한다면, 나무 길이를 상세 조정할 때 최악의 경우 1억번의 연산이 필요하기에 시간초과가 된 것 같다.

* 모든 나무를 잘라야 할 경우를 위해 미리 벡터 v에 0을 넣는다.
* 이후 cut_tree함수를 통해 n개의 나무 길이 중 정답과 접근한 나무의 길이를 찾는다.
* 이후 해당 길이를 1씩 늘리거나 줄이며 정답을 찾는다.





## 두 번째 풀이

```cpp
#include <bits/stdc++.h>
using namespace std;

long long  sum, cur, mid, high, low, n, m, k, max_value;
int ans;
vector<int> v;
void cut_tree() {
	while (high >= low) {
		sum = 0;
		mid = (high + low) / 2;
		for (auto a : v) {
			if (a <= mid) break;
			sum += a - mid;
		}
		if (sum < m) {
			high = mid - 1;
		}
		else if (sum > m) {
			low = mid + 1;
			ans = mid;
		}
		else if (sum == m) {
			ans = mid;
			return;
		}
	}
}
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n >> m;
	v.push_back(0);
	for (int i = 0; i < n; i++) {
		cin >> k;
		if (k > max_value) max_value = k;
		v.push_back(k);
	}
	sort(v.begin(), v.end(), greater<>());

	high = max_value;
	low = 0;
	cut_tree();
	cout << ans;
}
```

* 첫 번째 시도에서는 원소값들을 기준으로 근접한 원소값을 찾고 거기서 1씩 줄이거나 늘리면서 정답을 찾았는데 두 번째 시도에서는 0과 max사이의 모든 값을 바로 다 뒤졌다.
* 
