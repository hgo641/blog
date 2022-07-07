---
title: "백준 1213 팰린드롬 만들기"
date: 2022-07-06
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

임한수와 임문빈은 서로 사랑하는 사이이다.
<br/>
임한수는 세상에서 팰린드롬인 문자열을 너무 좋아하기 때문에, 둘의 백일을 기념해서 임문빈은 팰린드롬을 선물해주려고 한다.
<br/>
임문빈은 임한수의 영어 이름으로 팰린드롬을 만들려고 하는데, 임한수의 영어 이름의 알파벳 순서를 적절히 바꿔서 팰린드롬을 만들려고 한다.
<br/>
임문빈을 도와 임한수의 영어 이름을 팰린드롬으로 바꾸는 프로그램을 작성하시오.

<br/>
> 팰린드롬 : 앞으로 읽어도, 뒤로 읽어도 같은 문장

## 풀이

문장안에서 홀수개인 수가 두 개 이상이라면 팰린드롬을 만들 수 없다.<br/>

- 홀수개인 수가 0개일경우, 모든 수를 가장자리부터 하나씩 채운다.
- 홀수개인 수가 1개일 경우, 모든 수를 가장자리부터 하나씩 채우고 홀수개인 수를 문장의 가운데에 넣는다.

### 첫 번재 도전

```c++
#include<iostream>
#include<tuple>
#include<vector>
#include<string>
#include<queue>
#include <cstring>
#include <algorithm>
#include <map>
#include <stack>
using namespace std;

map<char, int> m;
int fail = 0;
string name;
int main() {
	ios_base::sync_with_stdio(false);cin.tie(NULL); cout.tie(NULL);
	cin >> name;

	for (auto n : name) {
		m[n] += 1;
	}
	int len = name.size();
	vector<char> v;
	stack<char> s;
	vector<char> odd;
	for (auto n : m) {
		if (n.second % 2 == 0) {
			for (int i = 0; i < n.second / 2; i++) {
				v.push_back(n.first);
				s.push(n.first);
			}
		}
		else {
			while (n.second--) {
				odd.push_back(n.first);
			}
			//fail 은 0또는 1
			fail += 1;
		}
		if (fail > 1) {
			cout << "I'm Sorry Hansoo" << "\n";
			break;
		}
	}
	if (fail <2) {
		if (fail == 1) {
			for (int i = 0; i < odd.size(); i++) {
				v.push_back(odd[i]);
			}
		}

		while (s.empty() != 1) {
			char a = s.top();
			s.pop();
			v.push_back(a);
		}
		for (int i = 0; i < v.size(); i++) {
			cout << v[i];
		}
	}
}
```

처음에 이렇게 풀었지만 정답이 여러 개일 경우, 사전순으로 앞서는 것을 출력한다는 것을 간과했다... 문제를 제대로 읽자
<br/>

### 두 번째 도전

```c++
int fail = 0;
string name;
string ans;
int alpha[26];
char mid;
int main() {
	ios_base::sync_with_stdio(false);cin.tie(NULL); cout.tie(NULL);
	cin >> name;
	for (auto n : name) {
		alpha[n - 'A'] += 1;
	}
	for (int i = 25; i >= 0; i--) {
		if (alpha[i] % 2 == 0) {
			for (int j = 0; j < alpha[i] / 2; j++) {
				ans = (char)(i + 'A') + ans;
			}
			for (int j = 0; j < alpha[i] / 2; j++) {
				ans += (char)(i + 'A');
			}
		}
		else {
			mid = (char)(i + 'A');
			fail += 1;
			alpha[i] -= 1;
			for (int j = 0; j < alpha[i] / 2; j++) {
				ans = (char)(i + 'A') + ans;
			}
			for (int j = 0; j < alpha[i] / 2; j++) {
				ans += (char)(i + 'A');
			}
		}
	}
	if (fail > 1) {
		cout << "I'm Sorry Hansoo\n";
	}
	else {
		if (fail == 0) {
			cout << ans;
		}
		else {
			int mididx = ans.size()/2;
			string prev = ans.substr(0, mididx);
			string next = ans.substr(mididx);
			ans = prev + mid + next;
			cout << ans;
		}
	}

}
```

팰린드롬이 여러 개일 경우, 사전순으로 앞서는 것을 먼저 출력해야한다. <br/>

* 그러므로, 각각의 알파벳에 배열 `alpha`의 인덱스 번호를 할당하고 수를 카운트한다.(A = 0 ~ Z = 25)<br/>

* 이후, 맨 뒤의 원소부터(Z to A) 차례대로 string `ans`의 가장자리에 채워넣는다. (`ans`는 출력할 팰린드롬을 의미한다.)

  > 첫 번째 풀이에서는 stack과 vector를 써서 풀었는데 ... 왜그랬을까?<br/>
  >
  >  바로 string에 추가해서 넣으면 된다... ㅎㅎ;

<br/>

* 만약 수의 카운트가 홀수라면, 팰린드롬의 가운데에 한 글자를 채워넣어야하므로 string `mid`에 저장해둔다.

  > 홀수개인 수들이 존재하는 만큼 fail을 증가시킨다. (변수명이 적절하지않은 것 같아서 찔린다)

<br/>

* 마지막으로 위에서 카운트한 fail이 2이상이면, 홀수개인 수가 2개 이상인 것이므로 팰린드롬 생성에 실패한다.

* fail이 0이면 가운데에 추가할 글자가 없으므로 바로 `ans`를 출력한다.

* fail이 1이면 `ans`를 반으로 나누고 가운데에 `mid`를 추가해서 출력한다.

  > 근데 저렇게 나눌 필요 없이<br/>
  >
  > `ans.insert(ans.begin(), ans.size()/2, mid);` 하면 한 줄로 되더라... ㅎㅎ!
