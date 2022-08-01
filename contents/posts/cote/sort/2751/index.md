---
title: "백준 2751 수 정렬하기 2"
date: 2022-08-01
tags:
  - cote
---

## 문제

N개의 수가 주어졌을 때, 이를 오름차순으로 정렬하는 프로그램을 작성하시오.
<br/>

## 첫 번째 풀이

sort함수를 이용하면 손쉽게 구현할 수 있다.

<br/>

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, k;
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	vector<int> v;
	for (int i = 0; i < n; i++) {
		cin >> k;
		v.push_back(k);
	}
	sort(v.begin(), v.end());
	for (int i = 0; i < n; i++) {
		cout << v[i] << "\n";
	}
}
```

```cpp
using namespace std;
bool num[2000001];
int main()
{

    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int N, number;
    cin >> N;

    while (N--) {
        cin >> number;
        num[number + 1000000] = true;
    }

    for (int i = 0; i <= 2000000; i++) {
        if (num[i]) {
            cout << i - 1000000 << "\n";
        }
    }

    return 0;
}
```

## 두 번째 풀이

배열의 인덱스를 활용한 풀이도 있었다! <br/>
세상엔 정말 똑똑한 사람들이 많은 것 같다. <br/><br/>

```cpp
#include <iostream>
using namespace std;
bool num[2000001];
int main()
{

    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int N, number;
    cin >> N;

    while (N--) {
        cin >> number;
        num[number + 1000000] = true;
    }

    for (int i = 0; i <= 2000000; i++) {
        if (num[i]) {
            cout << i - 1000000 << "\n";
        }
    }

    return 0;
}
```
