---
title: "백준 3986 좋은단어"
date: 2022-07-12
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

이번 계절학기에 심리학 개론을 수강 중인 평석이는 오늘 자정까지 보고서를 제출해야 한다. 보고서 작성이 너무 지루했던 평석이는 노트북에 엎드려서 꾸벅꾸벅 졸다가 제출 마감 1시간 전에 깨고 말았다. 안타깝게도 자는 동안 키보드가 잘못 눌려서 보고서의 모든 글자가 A와 B로 바뀌어 버렸다! 그래서 평석이는 보고서 작성을 때려치우고 보고서에서 '좋은 단어'나 세보기로 마음 먹었다.
<br/>
평석이는 단어 위로 아치형 곡선을 그어 같은 글자끼리(A는 A끼리, B는 B끼리) 쌍을 짓기로 하였다. 만약 선끼리 교차하지 않으면서 각 글자를 정확히 한 개의 다른 위치에 있는 같은 글자와 짝 지을수 있다면, 그 단어는 '좋은 단어'이다. 평석이가 '좋은 단어' 개수를 세는 것을 도와주자.<br/>

## 풀이

### 첫 번째 시도

문자열의 원소를 하나하나 확인하며 다음 과정을 진행한다.

- 벡터 v안에는 pair가 없는 문자들이 들어가서 대기하게 된다.
- `last`는 벡터 v의 마지막 원소이다. (v.back())
- 새로 들어오는 원소의 flag를 증가시킨다. (flag[0]은 A의 flag, flag[1]은 B의 flag이다.)
- 새로 들어오는 원소가 `last`와 같다면 벡터에서 `last`를 pop하고 해당 flag를 -2한다.
- `last`와 같지않다면 벡터안에 넣는다.

```c++
int n, cnt;
string str;
int flag[2];
vector<char> v;
char last = 'X';
int main() {
	cin >> n;
	while (n--) {
		cin >> str;
		for (int i = 0; i < str.size(); i++) {
			if (!v.empty()) last = v.back();
			else last = 'X';
			flag[str[i] - 'A']++;
			if (last == str[i]) {
				flag[str[i] - 'A'] -= 2;
				v.pop_back();
			}
			else {
				v.push_back(str[i]);
			}
		}
		if (v.empty()) cnt++;
		v.clear();
		flag[0] = 0;
		flag[1] = 0;
	}
	cout << cnt;
}
```

### 두 번째 시도

다른 분들의 풀이를 보며 굳이 flag로 확인할 필요가 없다는 걸 깨달았다...ㅎㅎ<br/>
위 코드를 단순화한 코드이다. \+ stack의 기능만으로 구현이 충분하므로 vector에서 stack으로 바꿔줬다.

<br/><br/>

```c++
int n, cnt;
string str;
stack<char> s;
int main() {
	cin >> n;
	while (n--) {
		cin >> str;
		for (auto c : str) {
			if (!s.empty() and s.top() == c) s.pop();
			else s.push(c);
		}
		if (s.empty()) cnt++;
		s = stack<char>();
	}
	cout << cnt;
}
```
