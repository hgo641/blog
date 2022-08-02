---
title: "백준 10816 숫자 카드 2"
date: 2022-08-02
tags:
  - cote
---

## 문제

[백준 10816 숫자 카드 2](https://www.acmicpc.net/problem/10816)
<br/>

## 첫 번째 풀이

map을 이용하면 간단하게 풀 수 있었다.

<br/>

```cpp
#include <bits/stdc++.h>
using namespace std;
int n, m, k;
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	map<int, int> card;
	for (int i = 0; i < n; i++) {
		cin >> k;
		card[k]++;
	}
	cin >> m;
	for (int i = 0; i < m; i++) {
		cin >> k;
		cout << card[k] << " ";
	}
}
```

<br/><br/>

> 그러나 시간이 872ms로 너무 오래 걸린 것 같아 더 좋은 풀이를 찾아보기로 했다... <br/>
> 시간이 100ms도 안걸린 풀이들이 있었지만, 너무 어나더 풀이같아서 해석하기가 두렵다 ㅎㅎ...<br/>
> 나도 언젠간 저렇게 버퍼를 쓰면서 풀 수 있을까...?ㅎ<br/>
> 그래서 일단은 이해도 잘가면서도 시간도 232ms밖에 안걸린 풀이를 적어보려고한다...

## 두 번째 풀이

```cpp
#include <bits/stdc++.h>
using namespace std;
int* a;
pair<int, int>* b;
int* c;
int n, m;

int main(void) {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	//가지고 있는 숫자 카드 입력
	cin >> n;
	a = new int[n];
	for (int i = 0; i < n; ++i) {
		cin >> a[i];
	}

	// 오름차순 정렬
	sort(a, a + n);


	cin >> m;
	// 카드 카운트를 위한 배열
	c = new int[m] {0, };

	// 후보 카드들 입력
	b = new pair<int, int>[m];
	for (int i = 0; i < m; ++i) {
		cin >> b[i].first;
		b[i].second = i;
	}

	// 오름차순 정렬
	sort(b, b + m);

	// 카운트 계산
	int j = 0;
	for (int i = 0; i < m; ++i) {

		// 후보 카드 넘버가 중복이라면 이전에 계산한 값을 바로 복사
		if (i > 0 && b[i].first == b[i - 1].first) {
			c[b[i].second] = c[b[i - 1].second];
			continue;
		}

		// 후보 카드 넘버가 실제 카드 넘버보다 작을 때까지 탐색 (만약 동일한 수가 있다면 b[i].first == a[j]가 된다.)
		while (j < n && b[i].first > a[j]) {
			++j;
		}

		// 카운트
		while (j < n && b[i].first == a[j]) {
			++j;
			++c[b[i].second];
		}
	}

	//출력하기
	for (int i = 0; i < m; ++i) {
		cout << c[i] << ' ';
	}

	return 0;
}
```
