---
title: "백준 10866 덱"
date: 2022-07-09
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

정수를 저장하는 덱(Deque)를 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.
<br/>
명령은 총 여덟 가지이다.
<br/>
push_front X: 정수 X를 덱의 앞에 넣는다.
push_back X: 정수 X를 덱의 뒤에 넣는다.
pop_front: 덱의 가장 앞에 있는 수를 빼고, 그 수를 출력한다. 만약, 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
pop_back: 덱의 가장 뒤에 있는 수를 빼고, 그 수를 출력한다. 만약, 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
size: 덱에 들어있는 정수의 개수를 출력한다.
empty: 덱이 비어있으면 1을, 아니면 0을 출력한다.
front: 덱의 가장 앞에 있는 정수를 출력한다. 만약 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
back: 덱의 가장 뒤에 있는 정수를 출력한다. 만약 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
<br/>

## 풀이

c++의 deque 라이브러리를 사용해 쉽게 구현할 수 있다.<br/>

```c++
int n, a;
string cmd;
deque<int> d;
int empty() {
	return d.empty();
}
int size() {
	return d.size();
}
int front() {
	if (d.empty()) return -1;
	else return d.front();
}
int back() {
	if (d.empty()) return -1;
	else return d.back();
}
void push_front() {
	cin >> a;
	d.push_front(a);
}
void push_back() {
	cin >> a;
	d.push_back(a);
}
int pop_back() {
	int top = back();
	if (top != -1) d.pop_back();
	return top;
}
int pop_front() {
	int top = front();
	if (top != -1) d.pop_front();
	return top;
}
int main() {
	//push_front push_back pop_front pop_back size empty front back
	ios_base::sync_with_stdio(false);cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	while (n--) {
		cin >> cmd;
		if (cmd == "push_front") push_front();
		else if (cmd == "push_back") push_back();
		else if (cmd == "pop_front") cout << pop_front() << "\n";
		else if (cmd == "pop_back") cout << pop_back() << "\n";
		else if (cmd == "size") cout << size() << "\n";
		else if (cmd == "empty") cout << empty() << "\n";
		else if (cmd == "front") cout << front() << "\n";
		else if (cmd == "back") cout << back() << "\n";
	}
}
```
