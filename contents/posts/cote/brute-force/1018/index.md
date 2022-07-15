---
title: "백준 1018 체스판 다시 칠하기"
date: 2022-07-13
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

지민이는 자신의 저택에서 MN개의 단위 정사각형으로 나누어져 있는 M×N 크기의 보드를 찾았다. 어떤 정사각형은 검은색으로 칠해져 있고, 나머지는 흰색으로 칠해져 있다. 지민이는 이 보드를 잘라서 8×8 크기의 체스판으로 만들려고 한다.
<br/><br/>
체스판은 검은색과 흰색이 번갈아서 칠해져 있어야 한다. 구체적으로, 각 칸이 검은색과 흰색 중 하나로 색칠되어 있고, 변을 공유하는 두 개의 사각형은 다른 색으로 칠해져 있어야 한다. 따라서 이 정의를 따르면 체스판을 색칠하는 경우는 두 가지뿐이다. 하나는 맨 왼쪽 위 칸이 흰색인 경우, 하나는 검은색인 경우이다.
<br/><br/>
보드가 체스판처럼 칠해져 있다는 보장이 없어서, 지민이는 8×8 크기의 체스판으로 잘라낸 후에 몇 개의 정사각형을 다시 칠해야겠다고 생각했다. 당연히 8\*8 크기는 아무데서나 골라도 된다. 지민이가 다시 칠해야 하는 정사각형의 최소 개수를 구하는 프로그램을 작성하시오.
<br/><br/>

## 풀이

만약 체스판이 "BWBW..."로 시작한다면 B의 x좌표 + y좌표는 언제나 짝수고 W의 x좌표 + y좌표는 언제나 홀수이다.<br/>
이를 이용해서 보드의 8x8만큼을 전부 순회하며, 해당 부분이 체스판이 된다면 얼마나 많은 색을 다시 칠해야하는지 카운트한다.<br/><br/>

```c++
int n,m;
char board[50][50];

int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);

	cin >> n >> m;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			cin >> board[i][j]; //보드 입력
		}
	}

	int count_bw = 0; // B로 시작하는 체스판
	int count_wb = 0; // W로 시작하는 체스판
	int ans = INT_MAX; // 출력값
	int min_cnt;

	for (int i = 0; i < n - 7; i++) {
		for (int j = 0; j < m - 7; j++) {
			for (int k = 0; k < 8; k++) {
				for (int l = 0; l < 8; l++) { // 보드의 8x8크기만큼 하나씩 비교
					char cur = board[i + k][j + l]; // 현재 보드판의 색
					if ((i + k + j + l) % 2) { //홀수
						if (cur != 'B') {
							count_wb++;
						}
						if (cur != 'W') {
							count_bw++;
						}
					}
					else { // 짝수
						if (cur != 'B') {
							count_bw++;
						}
						if (cur != 'W') {
							count_wb++;
						}
					}
				}
			}
			min_cnt = min(count_bw, count_wb); // 현재 보드의 8x8부분에서 색을 최소로 바꾸고 체스판을 만들 수 있는 수
			if (ans > min_cnt) { // min_cnt들 중, 가장 작은 min_cnt를 구해야 함
				ans = min_cnt;
			}
			count_bw = 0;
			count_wb = 0;
		}
	}
	cout << ans << "\n";
}
```
