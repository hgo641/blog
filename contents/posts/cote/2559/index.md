---
title: "백준 2559 수열"
date: 2022-06-29
tags:
  - cote
---

## 문제

매일 아침 9시에 학교에서 측정한 온도가 어떤 정수의 수열로 주어졌을 때, 연속적인 며칠 동안의 온도의 합이 가장 큰 값을 알아보고자 한다.
<br/>
예를 들어, 아래와 같이 10일 간의 온도가 주어졌을 때,
<br/>
3 -2 -4 -9 0 3 7 13 8 -3
<br/>
모든 연속적인 이틀간의 온도의 합은 아래와 같다.
<br/>

이때, 온도의 합이 가장 큰 값은 21이다.
<br/>
또 다른 예로 위와 같은 온도가 주어졌을 때, 모든 연속적인 5일 간의 온도의 합은 아래와 같으며,

<br/>

이때, 온도의 합이 가장 큰 값은 31이다.
<br/>
매일 측정한 온도가 정수의 수열로 주어졌을 때, 연속적인 며칠 동안의 온도의 합이 가장 큰 값을 계산하는 프로그램을 작성하시오.
<br/>

예를 들어, "Baekjoon Online Judge"를 ROT13으로 암호화하면 "Onrxwbba Bayvar Whqtr"가 된다. ROT13으로 암호화한 내용을 원래 내용으로 바꾸려면 암호화한 문자열을 다시 ROT13하면 된다. 앞에서 암호화한 문자열 "Onrxwbba Bayvar Whqtr"에 다시 ROT13을 적용하면 "Baekjoon Online Judge"가 된다.
<br/>

ROT13은 알파벳 대문자와 소문자에만 적용할 수 있다. 알파벳이 아닌 글자는 원래 글자 그대로 남아 있어야 한다. 예를 들어, "One is 1"을 ROT13으로 암호화하면 "Bar vf 1"이 된다.
<br/>

문자열이 주어졌을 때, "ROT13"으로 암호화한 다음 출력하는 프로그램을 작성하시오.
<br/>

## 풀이

부분합을 모두 구하고 배열 psum에 저장한다.
이후 for문을 통해 K길이만큼의 합을 구하고 그 중 최대값을 출력한다.

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int n, k, temp, psum[100001], ret = -1000000;
int main(){
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);cout.tie(NULL);
	cin >> n >> k;
	for(int i = 1; i <= n; i++){
		cin >> temp; psum[i] = psum[i - 1] + temp;
	}
	for(int i = k; i <= n; i++){
		ret = max(ret, psum[i] - psum[i - k]);
	}
	cout << ret << "\n";
    return 0;
}
```
