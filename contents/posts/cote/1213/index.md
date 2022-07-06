---
title: "백준 1213 팰린드롬 만들기"
date: 2022-07-06
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

임한수와 임문빈은 서로 사랑하는 사이이다.
<br/>
임한수는 세상에서 팰린드롬인 문자열을 너무 좋아하기 때문에, 둘의 백일을 기념해서 임문빈은 팰린드롬을 선물해주려고 한다.
<br/>
임문빈은 임한수의 영어 이름으로 팰린드롬을 만들려고 하는데, 임한수의 영어 이름의 알파벳 순서를 적절히 바꿔서 팰린드롬을 만들려고 한다.
<br/>
임문빈을 도와 임한수의 영어 이름을 팰린드롬으로 바꾸는 프로그램을 작성하시오.

<br/>
> 팰린드롬 : 앞으로 읽어도, 뒤로 읽어도 같은 문장

## 풀이

배열 cnt에 각각의 알파벳이 등장하는 카운트를 센다.<br/>

```c++
#include<bits/stdc++.h>
using namespace std;
string s, ret;
int cnt[200], flag;
char mid;
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cin >> s;
	for(char a : s)cnt[a]++;
	for(int i = 'Z'; i >= 'A'; i--){
		if(cnt[i]){
			//홀수인지확인
			if(cnt[i] & 1){
				mid = char(i);flag++;
				cnt[i]--;
			}
			if(flag == 2)break;
			for(int j = 0; j < cnt[i]; j += 2){
				ret = char(i) + ret;
				ret += char(i);
			}
		}
	}
	if(mid)ret.insert(ret.begin() + ret.size() / 2, mid);
	// flag가 2이면 카운트가 홀수인 수가 두 개 이상이라는 의미다. 홀수가 두 개 이상이라면 팰린드롬이 될 수 없다.
	if(flag == 2)cout << "I'm Sorry Hansoo\n";
	else cout << ret << "\n";
}
```
