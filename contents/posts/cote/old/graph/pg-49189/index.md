---
title: "프로그래머스 49189 가장 먼 노드"
date: 2022-06-27
tags:
  - cote
---

## 문제

n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.
<br/>

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.
<br/>

## 풀이

bfs를 사용해 구현

```cpp
#include<iostream>
#include<tuple>
#include<vector>
#include<string>
#include<queue>
#include <cstring>
#include <algorithm>
using namespace std;

queue<int> q;
int cur, answer;

int solution(int n, vector<vector<int>> edge) {
	answer = 0;
	vector <vector<int>> graph(n + 1);
	for (int i = 0; i < edge.size(); i++) {
		graph[edge[i][0]].push_back(edge[i][1]);
		graph[edge[i][1]].push_back(edge[i][0]);
	}
	q.push(1);
	vector<int> dist(n + 1, 0);
	dist[1] = 1;

	while (!q.empty()) {
		cur = q.front();
		q.pop();
		for (int i = 0; i < graph[cur].size(); i++) {
			if (dist[graph[cur][i]] == 0){
				q.push(graph[cur][i]);
				dist[graph[cur][i]] = dist[cur] + 1;
			}
		}
	}
	int far = *max_element(dist.begin(), dist.end());
	for (int i = 0; i < dist.size(); i++) {
		if (dist[i] == far) {
			answer++;
		}
	}
	return answer;
}
```
