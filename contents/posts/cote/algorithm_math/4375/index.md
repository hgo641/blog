---
title: "백준 4375 1"
date: 2022-07-13
tags:
  - cote
---

## 문제

2와 5로 나누어 떨어지지 않는 정수 n(1 ≤ n ≤ 10000)가 주어졌을 때, 1로만 이루어진 n의 배수를 찾는 프로그램을 작성하시오.
<br/>

> 배수는 어떤 수 n을 곱해서 만들 수 있는 수를 의미한다.
> 1로만 이루어진 n의 배수는 1, 11, 111, 1111 ... 을 의미한다
> <br/>

## 풀이

- 입력을 계속 받으므로 `while (cin >> n)`을 사용한다.
  > while(scanf(%d,&n) != EOF) 도 가능하다.
  > <br/><br/>

### 첫 번째 시도 - 시간초과

cmp는 배수를 의미한다. cmp가 n으로 나눠지지않는다면 1 -> 11 -> 111 ... 순으로 증가한다.<br/>
단순히 cmp = cmp \* 10 + 1 했을 때 시간초과가 나는 것을 볼 수 있다. cmp의 수가 너무 커졌기 때문으로 보인다. <br/>
cnt는 cmp의 자리수를 의미한다. <br/> <br/> <br/>

```cpp
int n;
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	while (cin >> n) {
		long long cmp = 1;
		int cnt = 1;
		while (cmp%n != 0) {
			cmp = cmp * 10 + 1;
			cnt += 1;
		}
		cout << cnt << "\n";
	}
}
```

### 두 번째 시도

mod 연산은 아래와 같은 공식으로 풀 수 있다. <br/><br/>

- (A+B)%N=(A%N+B%N)%N
- (A∗B)%N=(A%N)∗(B%N)%N
- (A−B)%N=((A%N)−(B%N)+N)%N
  <br/>
  우리는 ( cmp \* 10 + 1 ) % n한 값을 체크해야하므로, <br/><br/>

```cpp
while (cmp%n != 0) {
			cmp = cmp * 10 + 1;
			cnt += 1;
		}
```

<br/><br/>
위 부분을 다음과 같이 바꿔준다.<br/>

```cpp
while (cmp%n != 0) {
			cmp = (cmp%n) * (10%n) + 1;
			cnt += 1;
		}
```

<br/>
cmp에 %n연산을 추가하면 시간초과문제가 해결되는 것을 볼 수 있다.<br/>
전체 코드는 다음과 같다. <br/>

```cpp
int n;
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	while (cin >> n) {
		long long cmp = 1;
		int cnt = 1;
		while (cmp%n != 0) {
			cmp = (cmp%n) * (10%n) + 1;
			cnt += 1;
		}
		cout << cnt << "\n";
	}
}
```
