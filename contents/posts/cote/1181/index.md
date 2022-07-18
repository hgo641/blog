---
title: "백준 1181 단어 정렬"
date: 2022-07-18
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

알파벳 소문자로 이루어진 N개의 단어가 들어오면 아래와 같은 조건에 따라 정렬하는 프로그램을 작성하시오.
<br/><br/>

1. 길이가 짧은 것부터
2. 길이가 같으면 사전 순으로

## 첫 번째 풀이

중복을 제거해야하므로 set배열을 만들어 구현했다.<br/>
set내부에서 정렬이 안되는줄알고 sort를 하려고했지만, set은 자체적으로 오름차순으로 정렬을 해서 sort를 할 필요가 없었다.<br/><br/>

```c++
int n;
string str;
set<string> words[51]; //길이별로 문자열을 저장한다. (길이가 최대 50인 문자열만 들어온다)
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> str;
		words[str.size()].insert(str);
	}

	for (int i = 0; i < 51; i++) {
		//sort(words[i].begin(), words[i].end());
		for (auto w : words[i]) {
			cout << w << "\n"; // set은 자체적으로 오름차순 정렬을 하므로 작은 길이의 문자열들부터 차례대로 출력한다.
		}
	}
}
```

## 두 번째 풀이

set에 임의로 비교함수를 설정해서 길이가 짧은 순으로, 길이가 같다면 사전순으로 정렬되게 한다.<br/>
비교함수는 반환값이 true이면, 왼쪽이 오른쪽보다 먼저 나오게 해준다.<br/><br/>

```c++
struct Compare
{
	bool operator() (const string& _Left, const string& _Right) const {
		if (_Left.size() == _Right.size()) {
			return _Left < _Right;
		}
		else {
			return _Left.size() < _Right.size();
		}
	}
};

int n;
string str;
set<string, Compare> words;

int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> str;
		words.insert(str);
	}
	for (auto w : words) {
		cout << w << "\n";
	}
}
```
