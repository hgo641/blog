---
title: "백준 1620 나는야 포켓몬 마스터 이다솜"
date: 2022-06-29
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

안녕? 내 이름은 이다솜. 나의 꿈은 포켓몬 마스터야. 일단 포켓몬 마스터가 되기 위해선 포켓몬을 한 마리 잡아야겠지? 근처 숲으로 가야겠어.
<br/>
(뚜벅 뚜벅)
<br/>
얏! 꼬렛이다. 꼬렛? 귀여운데, 나의 첫 포켓몬으로 딱 어울린데? 내가 잡고 말겠어. 가라! 몬스터볼~
<br/>
(펑!) 헐랭... 왜 안 잡히지?ㅜㅜ 몬스터 볼만 던지면 되는 게 아닌가...ㅜㅠ

> 문제 설명이 엄청나게 긴 관계로 생략합니다... 이렇게 정성있는 문제는 처음이야
> <br/>
> <br/>
> . . .
> <br/>
> <br/>
> 오박사 : 그럼 다솜아 이제 진정한 포켓몬 마스터가 되기 위해 도감을 완성시키도록 하여라. 일단 네가 현재 가지고 있는 포켓몬 도감에서 포켓몬의 이름을 보면 포켓몬의 번호를 말하거나, 포켓몬의 번호를 보면 포켓몬의 이름을 말하는 연습을 하도록 하여라. 나의 시험을 통과하면, 내가 새로 만든 도감을 주도록 하겠네.

## 풀이

map을 사용해 string에 해당하는 index를 빠르게 찾을 수 있도록 한다.

```c++
#include<bits/stdc++.h>
using namespace std;
int n, m;
map<string, int> strmap;
string poke[100001];
string p;
int main() {
	ios_base::sync_with_stdio(false);cin.tie(NULL); cout.tie(NULL);
	cin >> n >> m;
	for (int i = 1; i <= n; i++) {
		cin >> p;
		poke[i] = p;
		strmap[p] = i;
	}
	for (int i = 0; i < m; i++) {
		cin >> p;
		auto find = strmap.find(p);
		if (find != strmap.end()) {
			cout << strmap[p] << "\n";
		}
		else {
			cout << (poke[stoi(p)]) << "\n";
		}
	}
}
```
