---
title: "백준 15686 치킨 배달"
date: 2022-09-24
tags:
  - cote
---

## 문제

[백준 15686 치킨 배달](https://www.acmicpc.net/problem/15686)

## 첫 번째 풀이 (실패)

bfs로 풀어봤다... `next_permunitation`으로 m개의 치킨집을 선택한다.<br/>
결과는 메모리초과로 실패였다.

```cpp
#include <string>
#include <vector>
#include <bits/stdc++.h>

using namespace std;
int n, d;
int graph[51][51];
vector<pair<int, int>> chicken;
vector<pair<int, int>> house;
int sum;
int dx[4] = { 0,0,-1,1 };
int dy[4] = { 1,-1,0,0 };
bool visited[51][51];
int bfs(pair<int,int> start) {
	queue<tuple<int, int, int>> q;
	int dist;
	q.push({ start.first, start.second,0 });
	while (q.size()) {
		int x, y;
		tie(x,y,dist) = q.front(); q.pop();
		visited[x][y] = 1;

		if (graph[x][y] == 2) {
			return dist;
		}

		for (int i = 0; i < 4; i++) {
			int cx = x + dx[i];
			int cy = y + dy[i];

			if (cx < 0 || cy < 0 || cx >= n || cy >= n || visited[cx][cy]) {
				continue;
			}

			q.push({ cx,cy, dist+1 });
		}
	}
	return dist;
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	cin >> n >> d;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			cin >> graph[i][j];
			if (graph[i][j] == 2) {
				chicken.push_back({ i, j });
			}
			if (graph[i][j] == 1) {
				house.push_back({ i, j });
			}
		}
	}

	int s = chicken.size();
	vector<bool>temp(s, 0);
	for (int i = 0; i < s-d; i++) {
		temp[s - 1 - i] = 1;
	}
	int idx;
	int minValue = 100000;

	do {
		vector<pair<int,int>> combi;
		for (int i = 0; i < temp.size(); i++) { // make combi
			if (temp[i]) {
				combi.push_back(chicken[i]);
			}
		}

		for (auto c : combi) { // off chicken
			graph[c.first][c.second] = 0;
		}

		// bfs
		int sum = 0;
		for (int i = 0; i < house.size(); i++) {
			memset(visited, 0, sizeof(visited));
			sum += bfs(house[i]);
		}
		minValue = minValue > sum ? sum : minValue;

		for (auto c : combi) { // on chicken
			graph[c.first][c.second] = 2;
		}

	} while (next_permutation(temp.begin(), temp.end()));

	cout << minValue;

}
```

## 두 번째 풀이

첫 번째 풀이에서는 집과 치킨집 사이의 거리를 bfs로 찾았지만 굳이 bfs로 풀 필요없이 집과 치킨집의 좌표를 가지고 거리를 구하는 방법이 있었다... 나는 바보?<br/>
비록 이중 for문으로 (치킨집개수 x 집 개수)만큼의 연산이 수행되지만, n의 크기는 최대 50개로 치킨집개수는 최대 49개, 집 개수는 최대 13개이므로 빠르게 수행이 가능하다.

```cpp
#include <string>
#include <vector>
#include <bits/stdc++.h>

using namespace std;
int n, d;
int graph[51][51];
vector<pair<int, int>> chicken;
vector<pair<int, int>> house;
int sum;
int dx[4] = { 0,0,-1,1 };
int dy[4] = { 1,-1,0,0 };


int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	cin >> n >> d;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			cin >> graph[i][j];
			if (graph[i][j] == 2) {
				chicken.push_back({ i, j });
			}
			if (graph[i][j] == 1) {
				house.push_back({ i, j });
			}
		}
	}

	int s = chicken.size();
	vector<bool>temp(s, 0);
	for (int i = 0; i < d; i++) {
		temp[s - 1 - i] = 1;
	}
	int idx;
	int minValue = 100000;

	do {
		vector<pair<int,int>> combi;
		for (int i = 0; i < temp.size(); i++) { // make combi
			if (temp[i]) {
				combi.push_back(chicken[i]);
			}
		}

		int sum = 0;
		for (int i = 0; i < house.size(); i++) {
			int minDis = 100;
			for (int j = 0; j < combi.size(); j++) {
				int xDis = abs(house[i].first - combi[j].first);
				int yDis = abs(house[i].second - combi[j].second);
				minDis = minDis > xDis + yDis ? xDis + yDis : minDis;
			}
			sum += minDis;
		}
		minValue = minValue > sum ? sum : minValue;


	} while (next_permutation(temp.begin(), temp.end()));

	cout << minValue;

}
```
