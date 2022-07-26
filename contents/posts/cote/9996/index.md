---
title: "백준 9996 한국이 그리울 땐 서버에 접속하지"
date: 2022-06-26
tags:
  - cote
series: "코테준비"
---

## 문제

선영이는 이번 학기에 오스트레일리아로 교환 학생을 가게 되었다.
<br/>
호주에 도착하고 처음 며칠은 한국 생각을 잊으면서 즐겁게 지냈다. 몇 주가 지나니 한국이 그리워지기 시작했다.
<br/>
선영이는 한국에 두고온 서버에 접속해서 디렉토리 안에 들어있는 파일 이름을 보면서 그리움을 잊기로 했다. 매일 밤, 파일 이름을 보면서 파일 하나하나에 얽힌 사연을 기억하면서 한국을 생각하고 있었다.
<br/>
어느 날이었다. 한국에 있는 서버가 망가졌고, 그 결과 특정 패턴과 일치하는 파일 이름을 적절히 출력하지 못하는 버그가 생겼다.
<br/>
패턴은 알파벳 소문자 여러 개와 별표(*) 하나로 이루어진 문자열이다.
<br/>
파일 이름이 패턴에 일치하려면, 패턴에 있는 별표를 알파벳 소문자로 이루어진 임의의 문자열로 변환해 파일 이름과 같게 만들 수 있어야 한다. 별표는 빈 문자열로 바꿀 수도 있다. 예를 들어, "abcd", "ad", "anestonestod"는 모두 패턴 "a*d"와 일치한다. 하지만, "bcd"는 일치하지 않는다.
<br/>
패턴과 파일 이름이 모두 주어졌을 때, 각각의 파일 이름이 패턴과 일치하는지 아닌지를 구하는 프로그램을 작성하시오.
<br/>

예를 들어, "Baekjoon Online Judge"를 ROT13으로 암호화하면 "Onrxwbba Bayvar Whqtr"가 된다. ROT13으로 암호화한 내용을 원래 내용으로 바꾸려면 암호화한 문자열을 다시 ROT13하면 된다. 앞에서 암호화한 문자열 "Onrxwbba Bayvar Whqtr"에 다시 ROT13을 적용하면 "Baekjoon Online Judge"가 된다.
<br/>

ROT13은 알파벳 대문자와 소문자에만 적용할 수 있다. 알파벳이 아닌 글자는 원래 글자 그대로 남아 있어야 한다. 예를 들어, "One is 1"을 ROT13으로 암호화하면 "Bar vf 1"이 된다.
<br/>

문자열이 주어졌을 때, "ROT13"으로 암호화한 다음 출력하는 프로그램을 작성하시오.
<br/>

## 풀이

find함수와 substr함수를 사용하면 쉽게 구현할 수 있다!

```c++
#include<iostream>
#include<tuple>
#include<vector>
#include<string>
using namespace std;
string str, input;
string prefix;
string suffix;
int cnt,pos;
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> cnt >> str;
	pos = str.find('*');
	prefix = str.substr(0, pos);
	suffix = str.substr(pos + 1);
	for (int i = 0; i < cnt; i++) {
		cin >> input;
		if (input.length() < prefix.length() + suffix.length()) { cout << "NE" << "\n"; continue; }
		if (input.substr(0, prefix.length()) == prefix && input.substr(input.length() - suffix.length()) == suffix) {
			cout << "DA" << "\n";
		}
		else cout << "NE" << "\n";
	}
}
```
