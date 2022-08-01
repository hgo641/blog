---
title: "백준 2309 일곱 난쟁이"
date: 2022-06-25
tags:
  - cote
---

## 문제

왕비를 피해 일곱 난쟁이들과 함께 평화롭게 생활하고 있던 백설공주에게 위기가 찾아왔다. 일과를 마치고 돌아온 난쟁이가 일곱 명이 아닌 아홉 명이었던 것이다.
<br/>
아홉 명의 난쟁이는 모두 자신이 "백설 공주와 일곱 난쟁이"의 주인공이라고 주장했다. 뛰어난 수학적 직관력을 가지고 있던 백설공주는, 다행스럽게도 일곱 난쟁이의 키의 합이 100이 됨을 기억해 냈다.
<br/>
아홉 난쟁이의 키가 주어졌을 때, 백설공주를 도와 일곱 난쟁이를 찾는 프로그램을 작성하시오.

<br/>

## 풀이

next_permutation함수를 이용하면 쉽게 풀 수 있다.

> next_permutation함수는 주어진 수들로 만들 수 있는 다음 순열을 반환한다.

```cpp

#include<bits/stdc++.h>

using namespace std;
int arr[9];
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
	for (int i=0; i<9; i++) cin >> arr[i];
	int sum = 0;
	do{
		sum = 0;
		for(int i =0; i<7; i++){
			sum = arr[i] + sum;
		}
		if (sum == 100) break;
	}while(next_permutation(arr, arr+9));

	sort(arr, arr+7);
	for(int i=0; i<7; i++) cout << arr[i]<<"\n";
}
```
