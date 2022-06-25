---
title: "백준 10988 팰린드롬인지 확인하기"
date: 2022-06-25
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

알파벳 소문자로만 이루어진 단어가 주어진다. 이때, 이 단어가 팰린드롬인지 아닌지 확인하는 프로그램을 작성하시오.

팰린드롬이란 앞으로 읽을 때와 거꾸로 읽을 때 똑같은 단어를 말한다.

level, noon은 팰린드롬이고, baekjoon, online, judge는 팰린드롬이 아니다.
<br/>

## 풀이

reverse를 사용한다.

```c++

#include<bits/stdc++.h>

using namespace std;

int main(){
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	string s;
	cin >> s;
	string r = s;
	reverse(r.begin(), r.end());
	if (r == s) cout << 1;
	else cout << 0;
}
```
