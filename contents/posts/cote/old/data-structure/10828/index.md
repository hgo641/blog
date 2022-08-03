---
title: "백준 10828 스택"
date: 2022-07-09
tags:
  - cote
---

## 문제

정수를 저장하는 스택을 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.
<br/>
명령은 총 다섯 가지이다.
<br/>
push X: 정수 X를 스택에 넣는 연산이다.
pop: 스택에서 가장 위에 있는 정수를 빼고, 그 수를 출력한다. 만약 스택에 들어있는 정수가 없는 경우에는 -1을 출력한다.
size: 스택에 들어있는 정수의 개수를 출력한다.
empty: 스택이 비어있으면 1, 아니면 0을 출력한다.
top: 스택의 가장 위에 있는 정수를 출력한다. 만약 스택에 들어있는 정수가 없는 경우에는 -1을 출력한다.
<br/>

## 풀이

c++의 stack라이브러리를 사용해 쉽게 구현할 수 있다.<br/>

```cpp
int n, a;
string cmd;
int main() {
	//push pop size empty top
	ios_base::sync_with_stdio(false);cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	stack<int> s;
	while (n--) {
		cin >> cmd;
		if (cmd == "push") {
			cin >> a;
			s.push(a);
		}
		else {
			if (cmd == "pop") {
				if (s.empty()) {
					cout << -1 << "\n";
				}
				else {
					int top = s.top();
					cout << top << "\n";
					s.pop();
				}
			}
			else if (cmd == "size") {
				cout << s.size() << "\n";
			}
			else if (cmd == "empty") {
				cout << s.empty() << "\n";
			}
			else {
				if (s.empty()) cout << -1 << "\n";
				else cout << s.top() << "\n";
			}
		}
	}
}
```
