---
title: "백준 2609 최대공약수와 최소공배수"
date: 2022-07-24
tags:
  - cote
series: "코테준비"
---

## 문제

두 개의 자연수를 입력받아 최대 공약수와 최소 공배수를 출력하는 프로그램을 작성하시오.

## 첫 번째 풀이

원시적인 방법으로 풀었다 \><<br/><br/>

### 최대 공약수

최대 공약수는 a또는 b보다 클 수 없다. <br/>
a > b이므로, i = 1 ~ b 사이의 모든 수로 a와 b를 나눈다.<br/><br/>

만약 a와 b 둘 다 i로 나눠진다면 gcd에 i를 대입한다.
가장 마지막에 대입된 값이 GCD(a,b)이다.

### 최소 공배수

역시나 원시적인 방법으로 a, b 두 수에 값(aq, bq)를 차례대로 곱하면서 a\*aq == b\*bq가 되는 aq와 bq를 구한다. <br/><br/>

최소 공배수는 최대 a\*b이므로 aq는 무조건 b이하의 수이고, bq는 a이하의 수이다.
aq가 b이하일동안 while문을 통해 aq와 bq를 1씩 더해가며 최소 공배수를 구한다.<br/><br/>

aq _ a와 bq _ b를 비교하며, aq\*a가 더 크다면 bq의 값을 1늘리고, bq\*b의 값이 더 크다면 aq의 값을 1늘린다.<br/><br/>

```c++
#include <bits/stdc++.h>
using namespace std;

int a, b, temp, q;
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> a >> b;
	if (a < b) { // 항상 a가 b보다 더 큼
		temp = a;
		a = b;
		b = temp;
	}
	int gcd, lcm;
	for (int i = 1; i <= b; i++) {
		if (a % i == 0 && b % i == 0) {
			gcd = i;
		}
	}
	int k = 1;
	int aq = 1;
	int bq = 1;
	while (aq <= b) {
		if (a * aq > b * bq) bq++;
		else if (a * aq < b * bq) aq++;
		else {
			lcm = a * aq;
			break;
		}
	}

	cout << gcd << "\n";
	cout << lcm << "\n";
}
```

## 두 번째 풀이

유클리드 알고리즘을 사용하면 빠르게 최대 공약수를 구할 수 있다!<br/><br/>
유클리드 알고리즘은 수 a, b가 있을 때 (이 때 a > b 이다.)

- 1. a를 b로 나눈 나머지인 r을 구하고
- 2. a에 b를 b에 r을 대입하고 다시 나머지를 구한다.
- 3. r이 0이 될때까지 2번을 반복한다.
- 4. r이 0이 되었을 때 b에 있는 값이 최대 공약수이다.
     <br/><br/>
     최소 공배수를 구하는 식은 다음과 같다.
     > (a \* b) / gcd(a,b)
     > <br/><br/>

```c++
#include <bits/stdc++.h>
using namespace std;

int a, b, temp, q;
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> a >> b;
	if (a < b) { // 항상 a가 b보다 더 큼
		temp = a;
		a = b;
		b = temp;
	}
	// 유클리드 호제법
	int r = 1;
	int _a = a;
	int _b = b;
	while (r != 0) {
		q = _a / _b;
		r = _a % (_b * q);
		_a = _b;
		_b = r;
	}

	int gcd = _a;
	int lcm = (a * b) / gcd;

	cout << gcd << "\n";
	cout << lcm << "\n";
}
```
