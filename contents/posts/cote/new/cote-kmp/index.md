---
title: "백준 11585 속타는 저녁 메뉴"
date: 2022-09-12
tags:
  - cote
---

## 문제

[백준 11585 속타는 저녁 메뉴](https://www.acmicpc.net/problem/11585)
<br/>

## 풀이

다른 kmp 알고리즘 문제와 풀이가 동일하지만, 이 문제의 경우 text가 원형으로 연결되어있기 때문에 text뒤에 text를 한 번 더 붙여줘야한다.

- abcde -> abcdeabcde
  그러나 위처럼 정직하게 text를 두 번 이어붙이면, pattern에 text의 맨 마지막 글자가 포함되는 경우 중복이 일어나서 카운트가 한 번 더 발생한다. 그러니 맨 마지막 글자를 제외하고 이어 붙여주면 된다.
- abcde -> abcdeabcd

<br/>

```cpp
#include <bits/stdc++.h>
#include <unordered_map>
using namespace std;

int N, cnt;
vector<char> target;
vector<char> str;

vector<int> getpi() {
	vector<int> pi(target.size(),0);
	for (int i = 1, j = 0; i < target.size(); i++) {
		while (j > 0 && target[i] != target[j]) {
			j = pi[j - 1];
		}
		if (target[i] == target[j]) {
			pi[i] = ++j;
		}
	}
	return pi;
}

void kmp() {
	vector<int> pi = getpi();
	for (int i = 0, j = 0; i < str.size(); i++) {
		while (j > 0 && str[i] != target[j]) {
			j = pi[j - 1];
		}
		if (str[i] == target[j]){
			if (j == target.size() - 1) {
				cnt++;
				j = pi[j - 1];
			}
			j++;
		}
	}
}

int gcd(int a, int b) { // a is bigger
	int temp;
	while (b > 0) {
		temp = b;
		b = a % b;
		a = temp;
	}
	return a;
}

int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);

	cin >> N;

	char c;
	for (int i = 0; i < N; i++) {
		cin >> c;
		target.push_back(c);
	}

	for (int i = 0; i < N; i++) {
		cin >> c;
		str.push_back(c);
	}

	for (int i = 0; i < N-1; i++) {
		str.push_back(str[i]);
	}

	kmp();

	int g = gcd(N, cnt);
	cout << cnt / g << "/" << N / g;

}
```
