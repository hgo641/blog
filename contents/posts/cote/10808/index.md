---
title: "백준 10808 알파벳 개수"
date: 2022-06-25
tags:
  - cote
series: "코테준비"
---

## 문제

알파벳 소문자로만 이루어진 단어 S가 주어진다. 각 알파벳이 단어에 몇 개가 포함되어 있는지 구하는 프로그램을 작성하시오.
<br/>

## 풀이

문자를 정수형으로 바꿔서 카운트한다.

```c++

#include<bits/stdc++.h>

using namespace std;
int arr[26] = {0,};
int main(){
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	string s;
	cin >> s;
	int n;
	for (char a:s){
		n = a - 'a';
		arr[n] = arr[n] + 1;
	}

	for(int i = 0; i<26; i++){
		cout << arr[i] << " ";
	}
}
```
