---
title: "백준 2213 트리의 독립집합 풀이"
date: 2023-01-18
tags:
  - cote
---

## 문제 : 트리의 독립집합

[백준 2213 트리의 독립집합](https://www.acmicpc.net/problem/2213)

> - 그래프 G(V, E)에서 정점의 부분 집합 S에 속한 모든 정점쌍이 서로 인접하지 않으면 (정점쌍을 잇는 간선이 없으면) S를 독립 집합(independent set)이라고 한다.
> - 가중치가 있는 트리에서 가장 큰 가중치의 합을 가지는 최대 독립 집합을 구해보자.

<br/>

이 문제에서의 가중치는 간선의 가중치가 아니라 노드가 가지는 가중치이다. 트리에서 서로 인접하지 않는 노드들을 골라 가중치의 합이 최대가 되는 집합을 구하면 되는 문제였다! 앞서 푼 사회망 서비스 문제와 유사한 문제였기에 어렵지않게 풀 수 있었다.

## 풀이 방법

이번 문제도 노드의 성질을 두 가지로 나눌 수 있다.

- 독립 집합으로 선택되거나,
- 선택되지 않거나!

dp\[node]가 node를 포함한 `sub graph`에서 얻을 수 있는 독립집합의 최대 가중치합이라고 하자. dp\[node]\[0]은 node가 선택되지 않았을 때의 최대 가중치합이고, dp\[node][1]은 node가 선택되었을 때의 최대 가중치합이라고 생각해보자!<br/>

또, 각 노드들의 가중치합은 cost\[node]로 표현해보자.

### 📌 어떤 노드 A를 선택할 경우

A를 선택했을 때의 dp는 위에서 정한대로, dp\[node][1]로 나타낼 수 있다.<br/>

dp\[node][1]은 노드 A를 선택한 경우이므로, A의 가중치합이 포함된다. (dp\[node][1] += cost\[node]) <br/>

또한 node의 `sub graph`에 포함된 노드들의 dp값도 전부 더해주어야 한다. A노드를 선택했으므로 A와 연결된 노드들은 독립집합의 조건에 의해 선택할 수 없다. <br/>

하위 노드들이 독립집합에 선택되지 않았을 때의 dp값을 전부 더해준다. (dp\[node][1] += dp\[child][0])

### 📌 어떤 노드 A를 선택하지 않을 경우

A를 선택하지 않았을 때의 dp는 위에서 정한대로, dp\[node][0]로 나타낼 수 있다.<br/>

마찬가지로 node의 `sub graph`에 포함된 노드들의 dp값도 전부 더해주어야 한다.<br/>

이 때, A는 선택되지 않았으므로 A와 연결된 노드들은 선택이 되었을 수도, 선택이 되지 않았을 수도 있다.<br/>

최대 가중치합을 구해야하므로, dp\[child][0]과 dp\[child][1] 중 더 큰 값을 dp\[node][0]에 더해준다. (dp\[node][0] += max(dp\[next][0], dp\[next][1]))

### 📌 독립 집합에 속하는 정점들 구하기

위에서 dp값을 정하는 과정을 통해 최대 독립 집합일 때의 가중치합은 구할 수 있다. 그러나 문제에서는 독립 집합에 포함되는 정점들이 무엇인지도 요구하고 있기 때문에 다시 dfs를 활용해서 집합에 속하는 정점들을 구했다.<br/>

모든 노드들의 dp값을 구한 상태에서, 이전 노드가 선택되었는지의 여부와 dp\[node][0]과 dp\[node][1]의 크기 비교를 통해 node가 독립 집합에 속하는 지를 체크했다. <br/>

- 이전 노드가 선택되었다면, 현재 노드는 독립집합의 조건에 의해 무조건 선택될 수 없다.
- 이전 노드가 선택되지 않았다면, 현재 노드는 선택되었을 수도 선택되지 않았을 수도 있다. 현재 노드의 dp\[node][1]과 dp\[node][0]의 값을 비교해 선택되었는지의 여부를 체크한다.

## 코드

```cpp
#include <bits/stdc++.h>

using namespace std;

int n, a, b;
int cost[10001];
vector<int> graph[10001];
int dp[10001][2];
bool visited[10001];
vector<int> route;

void setDP(int cur) {
	visited[cur] = 1;
	dp[cur][0] = 0;
	dp[cur][1] = cost[cur];
	for (int next : graph[cur]) {
		if (visited[next]) {
			continue;
		}
		setDP(next);
		dp[cur][0] += max(dp[next][0], dp[next][1]);
		dp[cur][1] += dp[next][0];
	}
}

void getRoute(int cur, bool prevState) {
	visited[cur] = 1;
	int curState = 0;

	if(prevState == 0) {
		if (dp[cur][1] > dp[cur][0]) {
			route.push_back(cur);
			curState = 1;
		}
	}

	for (int next : graph[cur]) {
		if (visited[next]) {
			continue;
		}
		getRoute(next, curState);
	}
}

 int main() {
	cin.tie(NULL); cout.tie(NULL); ios_base::sync_with_stdio(false);
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> cost[i];
	}
	for (int i = 0; i < n - 1; i++) {
		cin >> a >> b;
		graph[a].push_back(b);
		graph[b].push_back(a);
	}
	setDP(1);
	fill(&visited[0], &visited[10001], 0);
	getRoute(1, 0);
	cout << max(dp[1][0], dp[1][1]) << "\n";
	sort(route.begin(), route.end());
	for (int r : route) {
		cout << r << " ";
	}
}
```

## 트리에서의 다이나믹 프로그래밍!

문제 1 사회망 서비스를 풀 때 이걸 어떻게 다이나믹 프로그래밍으로 풀어야 할지 전혀 감이 잡히지 않았다...😭😭 그래도 문제를 풀면서

- 트리에서 노드의 상태가 나뉘고,
- 한 노드의 상태값에 따라, 연결된 다른 노드들의 상태값이 정해질 때

다이나믹 프로그래밍을 활용해서 문제를 풀 수 있음을 배웠다.<br/>

여러 문제들을 풀면서 다양한 풀이 방법을 생각해보도록 노력해야겠다.
