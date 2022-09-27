---
title: "백준 4179 불!"
date: 2022-09-27
tags:
  - cote
---

## 문제

[백준 4179 불!](https://www.acmicpc.net/problem/4179)

## 풀이

bfs를 사용하면 쉽게 풀 수 있는 문제였다. 지훈이가 걷는 루트와 불이 퍼지는 루트 두 개를 bfs로 체크한다. 지훈이의 다음 위치가 미로를 벗어나면 성공으로 간주한다.

```cpp
#include <bits/stdc++.h>

using namespace std;
int n,r,c;
char graph[1001][1001];
int dx[4] = { 0,0,-1,1 };
int dy[4] = { 1,-1,0,0 };

bool moveJh(vector<pair<int,int>> jh) {
	for (pair<int,int> j : jh) {
		int x = j.first; int y = j.second;
		for (int i = 0; i < 4; i++) {
			int cx = x + dx[i];
			int cy = y + dy[i];
			if (cx < 0 || cy < 0 || cx >= r || cy >= c) {
				return 1;
			}
			if (graph[cx][cy] == '#' || graph[cx][cy] == 'F' || graph[cx][cy] == 'J') {
				continue;
			}
			graph[cx][cy] = 'J';
		}
	}
	return 0;
}

void moveFire(vector<pair<int, int>> fire) {
	for (pair<int, int> f : fire) {
		int x = f.first; int y = f.second;
		for (int i = 0; i < 4; i++) {
			int cx = x + dx[i];
			int cy = y + dy[i];
			if (cx < 0 || cy < 0 || cx >= r || cy >= c || graph[cx][cy] == '#' ) {
				continue;
			}
			graph[cx][cy] = 'F';
		}
	}
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	cin >> r >> c;

	queue<pair<int, int>> jh;
	queue<pair<int, int>> fire;

	for (int i = 0; i < r; i++) {
		for (int j = 0; j < c; j++) {
			cin >> graph[i][j];
			if (graph[i][j] == 'J') {
				jh.push({ i,j });
			}
			if (graph[i][j] == 'F') {
				fire.push({ i,j });
			}
		}
	}

	int depth = 0;
	bool flag = 0;
	while (jh.size()) {
		queue<pair<int, int>> jtemp;
		queue<pair<int, int>> ftemp;
		flag = 0;
		while (fire.size()) {
			pair<int, int> f = fire.front(); fire.pop();
			int x = f.first; int y = f.second;
			for (int i = 0; i < 4; i++) {
				int cx = x + dx[i];
				int cy = y + dy[i];
				if (cx < 0 || cy < 0 || cx >= r || cy >= c || graph[cx][cy] == '#' || graph[cx][cy] == 'F') {
					continue;
				}
				graph[cx][cy] = 'F';
				ftemp.push({ cx,cy });
			}
		}
		fire = ftemp;

		while (jh.size()) {
			pair<int, int> j = jh.front(); jh.pop();
			int x = j.first; int y = j.second;
			for (int i = 0; i < 4; i++) {
				int cx = x + dx[i];
				int cy = y + dy[i];
				if (cx < 0 || cy < 0 || cx >= r || cy >= c) {
					flag = 1;
				}
				if (graph[cx][cy] == '#' || graph[cx][cy] == 'F' || graph[cx][cy] == 'J') {
					continue;
				}
				graph[cx][cy] = 'J';
				jtemp.push({ cx,cy });
			}
		}
		jh = jtemp;
		depth++;
		if (flag) {
			cout << depth;
			break;
		}

	}
	if (!flag) {
		cout << "IMPOSSIBLE";
	}
}
```
