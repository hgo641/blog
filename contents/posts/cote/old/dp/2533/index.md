---
title: "백준 2533 사회망 서비스(SNS) 풀이"
date: 2023-01-17
tags:
  - cote
---

## 문제1 : 사회망 서비스

[백준 2533 사회망 서비스(SNS)](https://www.acmicpc.net/problem/2533)

> - SNS의 친구 관계를 트리형태인 친구 관계 그래프로 표현한다.
> - 친구 관계 그래프에 속한 사람은 얼리어답터이거나 얼리어답터가 아니다.
> - 얼리 아답터가 아닌 사람들은 자신의 모든 친구들이 얼리 아답터일 때만 아이디어를 받아들인다.
> - 그래프안의 모든 사람들이 아이디어를 받아들이기 위해 필요한 최소 얼리어답터의 수를 구한다.

<br/>

친구 관계 그래프가 아래와 같을 경우, `2, 3, 4`번이 얼리어답터라면 그래프내의 모든 사람들이 아이디어를 받아들일 수 있다.

![](boj-sns.png)

<br/>

## 풀이 방법

풀이 방법은 생각해봐도 모르겠어서 다른 분들의 풀이를 참고했다. 😋

- 각 노드들의 성질은 두 가지로 분리할 수 있다. (얼리어답터이거나, 얼리어답터가 아니거나.)
- dp[node]가 node를 포함한 `sub graph`내의 모든 노드들이 아이디어를 전파받기 위한 최소 얼리어답터의 개수라고 해보자.
- node는 두 가지의 성질을 가질 수 있으므로, dp\[node][0]과 dp\[node][1]로 나눌 수 있다.
  - dp\[node][0] 은 node가 얼리어답터가 아닐 경우, node를 포함한 `sub graph`내의 모든 노드들이 아이디어를 전파받기 위한 최소 얼리어답터의 개수로 보고,
  - dp\[node][1]은 node가 얼리어답터일 경우, node를 포함한 `sub graph`내의 모든 노드들이 아이디어를 전파받기 위한 최소 얼리어답터의 개수로 보자!

### 📌 어떤 노드 A가 얼리어답터가 아닐 경우

A와 연결된 모든 친구들이 얼리어답터이어야 아이디어를 받아들일 수 있다! <br/>

즉, A가 얼리어답터가 되기 위해서 필요한 최소 얼리어답터 개수는 연결된 노드들이 얼리어답터일 때 필요한 최소 얼리어답터 개수들의 합이다.<br/>

코드로 나타내면 이와 같다. dp\[node][0] += dp\[child][1]

### 📌 어떤 노드 A가 얼리어답터인 경우

A가 얼리어답터이므로, A와 연결된 친구들이 얼리어답터여도, 얼리어답터가 아니어도 A는 아이디어를 전파받는다.<br/>

즉, A를 포함한 `sub graph`내의 모든 노드들이 얼리어답터가 되기 위해서 필요한 최소 얼리어답터 개수는 dp\[node][1] += min(dp\[child][0], dp\[child][1])이 된다.

## 코드

```cpp
#include <bits/stdc++.h>

using namespace std;

int n, m, a, k, b, t;
int ans;

vector<int> graph[1000001];
bool visited[1000001];
int dp[1000001][2];

void go(int num) {
	visited[num] = 1;
	dp[num][0] = 0;
	dp[num][1] = 1;

	for (int next : graph[num]) {
		if (visited[next]) {
			continue;
		}
		go(next);
		dp[num][0] += dp[next][1];
		dp[num][1] += min(dp[next][0], dp[next][1]);
	}
}

 int main() {
	cin.tie(NULL); cout.tie(NULL); ios_base::sync_with_stdio(false);
	cin >> n;
	for (int i = 0; i < n - 1; i++) {
		cin >> a >> b;
		graph[a].push_back(b);
		graph[b].push_back(a);
	}
	go(1);
	cout << min(dp[1][0], dp[1][1]);
}

```

## 문제 2 : 트리의 독립집합

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
