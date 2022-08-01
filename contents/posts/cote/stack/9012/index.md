---
title: "백준 9012 괄호"
date: 2022-08-01
tags:
  - cote
---

## 문제

괄호 문자열(Parenthesis String, PS)은 두 개의 괄호 기호인 ‘(’ 와 ‘)’ 만으로 구성되어 있는 문자열이다. 그 중에서 괄호의 모양이 바르게 구성된 문자열을 올바른 괄호 문자열(Valid PS, VPS)이라고 부른다. 한 쌍의 괄호 기호로 된 “( )” 문자열은 기본 VPS 이라고 부른다. 만일 x 가 VPS 라면 이것을 하나의 괄호에 넣은 새로운 문자열 “(x)”도 VPS 가 된다. 그리고 두 VPS x 와 y를 접합(concatenation)시킨 새로운 문자열 xy도 VPS 가 된다. 예를 들어 “(())()”와 “((()))” 는 VPS 이지만 “(()(”, “(())()))” , 그리고 “(()” 는 모두 VPS 가 아닌 문자열이다.
<br/><br/>
여러분은 입력으로 주어진 괄호 문자열이 VPS 인지 아닌지를 판단해서 그 결과를 YES 와 NO 로 나타내어야 한다.
<br/>

## 풀이

스택의 원리를 사용하면 쉽게 풀 수 있다.<br/>
스택으로 구현해도 되지만 int형 변수 하나로도 VPS를 판별할 수 있다.<br/>
'(' 와 ')'의 짝이 맞지않으면 VPS가 아니다. <br/>
'('가 먼저 있어야하므로 문자 '('를 카운트하고 ')'가 들어올 때 마다 카운트를 하나씩 빼주면 된다.<br/>
카운트가 0보다 크다면 짝이 맞지않은 것이므로 VPS가 아니다.<br/>
<br/>
이를 코드로 구현하면 아래와 같다.

<br/>

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, flag;
string str;
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	for (int i = 0; i < n; i++) {
		flag = 0;
		cin >> str;
		for (int j = 0; j < str.size(); j++) {
			if (str[j] == ')') {
				if (flag) {
					flag--;
				}
				else {
					flag = 1;
					break;
				}
			}
			else if (str[j] == '(') {
				flag++;
			}

		}
		if (flag) {
			cout << "NO" << "\n";
		}
		else cout << "YES" << "\n";
	}
}
```
