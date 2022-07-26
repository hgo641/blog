---
title: "백준 11655 ROT13"
date: 2022-06-25
tags:
  - cote
series: "코테준비"
---

## 문제

ROT13은 카이사르 암호의 일종으로 영어 알파벳을 13글자씩 밀어서 만든다.
<br/>

예를 들어, "Baekjoon Online Judge"를 ROT13으로 암호화하면 "Onrxwbba Bayvar Whqtr"가 된다. ROT13으로 암호화한 내용을 원래 내용으로 바꾸려면 암호화한 문자열을 다시 ROT13하면 된다. 앞에서 암호화한 문자열 "Onrxwbba Bayvar Whqtr"에 다시 ROT13을 적용하면 "Baekjoon Online Judge"가 된다.
<br/>

ROT13은 알파벳 대문자와 소문자에만 적용할 수 있다. 알파벳이 아닌 글자는 원래 글자 그대로 남아 있어야 한다. 예를 들어, "One is 1"을 ROT13으로 암호화하면 "Bar vf 1"이 된다.
<br/>

문자열이 주어졌을 때, "ROT13"으로 암호화한 다음 출력하는 프로그램을 작성하시오.
<br/>

## 풀이

아스키코드값을 활용한다.

> A ~ Z 는 65 ~ 90
> a ~ z 는 97 ~ 122

```c++
#include<iostream>
#include<tuple>
#include<vector>
#include<string>
using namespace std;

char c;
string str;
int n;
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	getline(cin,str);
	for (int i = 0; i < str.length(); i++) {
		n = (int)str[i];
		if (n >= 97 && n <= 122) {
			// a b c d e f g h i j k l m n o p q r s t u v w x y z
			n += 13;
			n = (n>122)?n%123 + 97:n;
			str[i] = (char)n;
		}
		else if (n >= 65 && n <= 90) {
			n += 13;
			n = (n > 90) ? n % 91 + 65 : n;
			str[i] = char(n);
		}
	}
	cout << str;
}
```
