---
title: "백준 1629 곱셈"
date: 2022-07-12
tags:
  - cote
---

## 문제

자연수 A를 B번 곱한 수를 알고 싶다. 단 구하려는 수가 매우 커질 수 있으므로 이를 C로 나눈 나머지를 구하는 프로그램을 작성하시오.
<br/>
첫째 줄에 A, B, C가 빈 칸을 사이에 두고 순서대로 주어진다. A, B, C는 모두 2,147,483,647 이하의 자연수이다..<br/>

## 풀이

간단한 문제인 것 같지만, 수들이 2,147,483,647로 크기가 크므로 그냥 곱하면 오버헤드가 일어난다.<br/>
`분할정복 거듭제곱`을 이용하면 오버헤드없이 계산을 할 수 있다.<br/>
![](algorithm.png)

### 첫 번째 시도

위 공식을 재귀함수로 구현했다.
mod 연산을 마지막에 하지않고 각 원소들을 곱셈연산하기전 mod를 먼저하고 곱했다.

```cpp
typedef long long ll;
ll a, b, c;
int mod(ll x, ll y) {
	if (y == 1) {
		return x%c;
	}
	ll C = mod(x, y/2);
	if (y % 2) { //홀수
		return (C%c *C%c*x%c);
	}
	else {
		return (C%c * C%c);
	}
}
int main() {
	cin >> a >> b >> c;
	cout << mod(a, b);
}
```

### 두 번째 시도

컴퓨터 보안시간에 `Fast Modular Exponentiation Algorithm`을 배워 적용해봤다!

> 보안 공부한걸 활용할 수 있어 기쁘다... 흑흑
> <br/><br/>

- `#include <bitset>`을 해서 bitset 라이브러리를 가져온다.
- bitset을 사용해서 10진수 모듈러인 b를 2진수로 변환한다.
- 변환한 2진수를 가지고 `Fast Modular Exponentiation Algorithm`을 적용한다.

```c++
typedef long long ll;
ll a, b, c, f;

int main() {
	cin >> a >> b >> c;
	bitset<100> bs(b);
	string s = bs.to_string();
	s = s.substr(s.find('1'));

	f = 1;
	for (int i = 0; i < s.size(); i++) {
		f = (f * f) % c;
		if (s[i] == '1') {
			f = (f * a) % c;
		}
	}
	cout << f;
}
```
