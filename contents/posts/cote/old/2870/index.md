---
title: "백준 2870 수학숙제"
date: 2022-09-16
tags:
  - cote
---

## 문제

[백준 2870 수학숙제](https://www.acmicpc.net/problem/2870)

## 풀이

추출할 숫자는 최대 100글자이기에 int, long long과 같이 숫자를 담는 자료형으로 담으면 오버헤드가 일어나기에 숫자를 문자열로 담아야한다. <br/>
sort()에 문자열이 오름차순으로 정렬되도록 cmp함수를 만들어 인자로 넣는다.<br/>
문자열로 숫자를 추출한 뒤 숫자의 맨 앞에 있는 "0"을 제거하는 과정이 필요하다. 만약 문자열이 "0"으로만 이루어져있다면 "0"이 하나만 남게 한다. 자세한 코드는 아래와 같다.

```cpp
#include <bits/stdc++.h>
using namespace std;
#define sz(x) ((int)(x).size())
#define f first
#define s second
typedef unsigned long long ll;
const int INF = 987654321;
int n;
vector<string> v;
string s, ret;
void go(){
	if(ret.size()){
		while(true){
			if(ret.size() && ret.front() == '0')ret.erase(ret.begin());
			else break;
		}
		if(ret.size() == 0) ret = "0";
		v.push_back(ret);
		ret = "";
	}
}
bool cmp(string a, string b){
	if(a.size() == b.size()) return a < b;
	return a.size() < b.size();
}
int main () {
	cin >> n;
	for(int i = 0; i < n; i++){
		cin >> s;
	 	ret = "";
		for(int j = 0; j < s.size(); j++){
			if(s[j] < 58)ret += s[j];
			else{
				go();
			}
		}
		go();
	}
	sort(v.begin(), v.end(), cmp);
	for(string i : v)cout << i << "\n";
	return 0;
}
```
