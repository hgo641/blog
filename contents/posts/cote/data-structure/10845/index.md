---
title: "백준 10828 스택"
date: 2022-07-09
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

정수를 저장하는 큐를 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.
<br/>
명령은 총 여섯 가지이다.
<br/>
push X: 정수 X를 큐에 넣는 연산이다.
pop: 큐에서 가장 앞에 있는 정수를 빼고, 그 수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
size: 큐에 들어있는 정수의 개수를 출력한다.
empty: 큐가 비어있으면 1, 아니면 0을 출력한다.
front: 큐의 가장 앞에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
back: 큐의 가장 뒤에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
<br/>

## 풀이

c++의 stack라이브러리를 사용해 쉽게 구현할 수 있다.<br/>

```c++
int n, a;
string cmd;
int main() {
	//push pop size empty front back
	ios_base::sync_with_stdio(false);cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	queue<int> q;
	while (n--) {
		cin >> cmd;
		if (cmd == "push") {
			cin >> a;
			q.push(a);
		}
		else {
			if (cmd == "pop") {
				if (q.empty()) {
					cout << -1 << "\n";
				}
				else {
					int top = q.front();
					cout << top << "\n";
					q.pop();
				}
			}
			else if (cmd == "size") {
				cout << q.size() << "\n";
			}
			else if (cmd == "empty") {
				cout << q.empty() << "\n";
			}
			else if (cmd == "front"){
				if (q.empty()) cout << -1 << "\n";
				else cout << q.front() << "\n";
			}
			else{
				if (q.empty()) cout << -1 << "\n";
				else cout << q.back() << "\n";
			}
		}
	}
}
```
