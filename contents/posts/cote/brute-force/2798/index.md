---
title: "백준 2798 블랙잭"
date: 2022-07-17
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

카지노에서 제일 인기 있는 게임 블랙잭의 규칙은 상당히 쉽다. 카드의 합이 21을 넘지 않는 한도 내에서, 카드의 합을 최대한 크게 만드는 게임이다. 블랙잭은 카지노마다 다양한 규정이 있다.
<br/><br/>
한국 최고의 블랙잭 고수 김정인은 새로운 블랙잭 규칙을 만들어 상근, 창영이와 게임하려고 한다.
<br/><br/>
김정인 버전의 블랙잭에서 각 카드에는 양의 정수가 쓰여 있다. 그 다음, 딜러는 N장의 카드를 모두 숫자가 보이도록 바닥에 놓는다. 그런 후에 딜러는 숫자 M을 크게 외친다.
<br/><br/>
이제 플레이어는 제한된 시간 안에 N장의 카드 중에서 3장의 카드를 골라야 한다. 블랙잭 변형 게임이기 때문에, 플레이어가 고른 카드의 합은 M을 넘지 않으면서 M과 최대한 가깝게 만들어야 한다.
<br/><br/>
N장의 카드에 써져 있는 숫자가 주어졌을 때, M을 넘지 않으면서 M에 최대한 가까운 카드 3장의 합을 구해 출력하시오.

## 풀이

`next_permutation` 함수를 이용해 조합을 구현해서 풀었다<br/><br/>
조합으로 카드를 세 개씩 임의로 뽑고 세 개의 숫자합을 구하며 가장 정답에 가까운 숫자합을 도출한다. <br/><br/><br/>

```c++
int n,m, ans, sum;
int card[100];
int k = 3;

int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n >> m;
	for (int i = 0; i < n; i++) {//초기 카드 입력
		cin >> card[i];
	}

	vector<int> flag;
	for (int i = 0; i < n - k; i++) {// 전체 카드 수 - 뽑아야 하는 카드 수(3개)만큼 0을 flag에 추가 == 뽑지않는 카드 수
		flag.push_back(0);
	}

	while (k--) { // 뽑아야 하는 카드 수만큼(3개) 1을 flag에 추가
		flag.push_back(1);
	}

	do {
		sum = 0;
		for (int i = 0; i < n; i++) {
			if (flag[i] == 1) { // flag에서 1이 있는 i번째는 더하고, 0인 i번째는 더하지않는다.
				sum += card[i];
			}
		}
		if (m - sum >= 0) { // 합이 m을 넘으면 안됨
			if (sum > ans) { // 합이 기존 ans보다 더 m에 가까운 수인지 확인
				ans = sum;
			}
		}
	} while (next_permutation(flag.begin(), flag.end())); // next_permutation 함수를 이용해 flag원소들로 순열을 만듦
	cout << ans;
}
```
