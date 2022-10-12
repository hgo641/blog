---
title: "cpp 우선순위큐로 다익스트라 구현하기"
date: 2022-10-11
tags:
  - cpp
---



## 최단경로 문제 풀어보기

[백준 1753 최단경로](https://www.acmicpc.net/problem/1753) <br/>

주어진 시작점에서 다른 모든 정점으로의 최단 경로를 구하는 문제이다.



### 첫 번째 풀이 😥

```cpp
#include <bits/stdc++.h>
using namespace std;

int V, E, s, u, v, w;
int cur;
vector<pair<int, int>> graph[20001];
int dp[20001];
queue<int> q;

int main() {
	ios::sync_with_stdio(0);
	cin.tie(NULL);
	cout.tie(NULL);
	memset(dp, -1, sizeof(dp));
	cin >> V >> E;
	cin >> s;
	for (int i = 0; i < E; i++) { // 그래프 입력
		cin >> u >> v >> w; // src, dest, weight
		graph[u].push_back({ v,w });
	}
    
	q.push(s); dp[s] = 0; // 큐에 시작점을 넣음
	while (q.size()) { // bfs로  
		cur = q.front(); q.pop();
		for (pair<int, int> next : graph[cur]) {
			v = next.first;
			w = next.second;
			if (dp[v] != -1 && dp[v] < (w + dp[cur])) {
				continue;
			}
			dp[v] = w + dp[cur];
			q.push(v);
		}
	}
	for (int i = 1; i <= V; i++) {
		if (dp[i] == -1) {
			cout << "INF" << "\n";
			continue;
		}
		cout << dp[i] << "\n";
	}
	
}
```



## 다익스트라!

가중치 그래프에서 최단 거리를 구하는 알고리즘이다.

* 가중치중에 음수가 있으면 사용할 수 없다.

```
1. 방문이 가능하며, 아직 방문하지 않은 정점 중 가장 가중치값이 작은 정점을 방문한다.(처음은 시작 노드)
2. 해당 정점을 거쳐서 갈 수 있는 정점의 거리가 이전에 기록한 값보다 작다면 그 거리를 갱신한다. (초기에 모든 거리는 무한으로 초기화)
```



### 우선순위큐를 활용한 풀이 😀

```cpp
#include <bits/stdc++.h>
using namespace std;

int V, E, s, u, v, w;
pair<int,int> cur;
vector<pair<int, int>> graph[20001];
int dp[20001];
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> q;

int main() {
	ios::sync_with_stdio(0);
	cin.tie(NULL);
	cout.tie(NULL);
	memset(dp, -1, sizeof(dp));
	cin >> V >> E;
	cin >> s;
	for (int i = 0; i < E; i++) {
		cin >> u >> v >> w;
		graph[u].push_back({ v,w });
	}
	q.push({ 0,s });
	dp[s] = 0;
	while (q.size()) {
		cur = q.top(); q.pop();
		if (cur.first != dp[cur.second]){
			continue;
		}
		for (pair<int, int> next : graph[cur.second]) {
			v = next.first;
			w = next.second;
			if (dp[v] != -1 && dp[v] < (w + dp[cur.second])) {
				continue;
			}
			dp[v] = w + dp[cur.second];
			q.push({dp[v],v});
		}
	}
	for (int i = 1; i <= V; i++) {
		if (dp[i] == -1) {
			cout << "INF" << "\n";
			continue;
		}
		cout << dp[i] << "\n";
	}
	
}
```

