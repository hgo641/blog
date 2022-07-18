---
title: "백준 1260 DFS와 BFS"
date: 2022-07-10
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.
<br/>

## 풀이

dfs - 재귀함수로 구현
bfs - queue를 가지고 구현

```c++
#include<iostream>
#include<tuple>
#include<vector>
#include<string>
#include<queue>
#include <cstring>
#include <algorithm>
using namespace std;
int n, m, v;
vector<int> graph[1001];
bool visited[1001];
queue<int> q;
void dfs(int node) {
	for (int i = 0; i < graph[node].size(); i++) {
		if (visited[graph[node][i]] == 0) {
			cout << graph[node][i] << " ";
			visited[graph[node][i]] = 1;
			dfs(graph[node][i]);
		}
	}
}

void bfs(int node) {
	q.push(v);
	visited[v] = 1;
	while (!q.empty()) {
		int first = q.front();
		cout << first << " ";
		q.pop();
		for (int i = 0; i < graph[first].size(); i++) {
			if (visited[graph[first][i]] == 0) {
				visited[graph[first][i]] = 1;
				q.push(graph[first][i]);
			}
		}
	}
}

int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n >> m >> v;
	int a, b;
	//간선 입력
	for (int i = 0; i < m; i++) {
		cin >> a >> b;
		graph[a].push_back(b);
		graph[b].push_back(a);
	}
	// vector sort
	for (int i = 1; i < n + 1; i++) {
		sort(graph[i].begin(), graph[i].end());
	}
	cout << v << " ";
	visited[v] = 1;
	dfs(v);
	// visited init
	cout << "\n";
	memset(visited,0,sizeof(visited));
	bfs(v);
}
```
