---
title: "백준 1325 효율적인 해킹"
date: 2022-09-22
tags:
  - cote
---

## 문제

[백준 1325 효율적인 해킹](https://www.acmicpc.net/problem/1325)
<br/>

## 첫 번째 풀이(실패)

- 배열 dp로 노드의 깊이를 카운트한다. dp[i]는 i번째 노드의 깊이를 value로 가진다.
- bfs를 하면visited로 해당 노드의 방문 여부를 체크하는게 원칙이지만, dp로 방문 여부 체크를 동시에 할 수 있을 것이라 생각해 visited배열을 만들지않았다.
- - dp를 -1로 초기화해서 탐색하려는 노드의 dp가 -1일 경우는, 아직 visit하지 않았다고 보고, 0이상인 경우는 visit했다고 체크하게 했다.

```cpp
#include <string>
#include <vector>
#include <bits/stdc++.h>

using namespace std;

vector<int> v[10001];
int dp[10001];
int n, m;
int maxValue = 0;

void dfs(int k) {
	if (dp[k] != -1) {
		return;
	}

	if (v[k].size() == 0) {
		dp[k] = 0;
		return;
	}

	for (int i = 0; i < v[k].size(); i++) {
		int cur = v[k][i];
		dfs(cur);
		dp[k] = dp[cur] + 1 > dp[k] ? dp[cur] + 1 : dp[k];
		if (maxValue < dp[k]) {
			maxValue = dp[k];
		}
	}
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	cin >> n >> m;
	for (int i = 0; i <= n; i++) {
		dp[i] = -1;
	}
	int a, b;
	for (int i = 0; i < m; i++) {
		cin >> a >> b;
		v[b].push_back(a);
	}
	for (int i = 1; i <= n; i++) {
		dfs(i);
	}


	for (int i = 1; i <= n; i++) {
		if (dp[i] == maxValue) {
			cout << i <<" ";
		}
	}

}
```

<br/>
* 결과는...? 두둥둥 "메모리 초과"....
* 메모리 초과가 난 이유는 아래와 같이 cyclic graph일 경우 무한 루프가 생기기 때문이다.
![](cyclic-graph.png)
<br/>
0번 노드에서 탐색이 시작됐다고 치자. dp[0]은 아직 -1이다. 그런데 0, 1, 2번 노드 사이에 사이클이 있으므로, 0번에서 1번에서 넘어가고 1번에서 2번노드로 들어왔을 때 다시 0번 노드를 체크하게 된다. 이 때 dp[0]이 -1이기때문에 또 0번 노드를 재호출하며 무한 루프에 빠지게 된다... 
* 왜 시간초과가 아니라 메모리 초과인가? 하는 의문이 생길수도 있는데(바로 나) 이 문제의 경우는 2초로 시간 제한이 넉넉했다! 그렇기에 무한루프로 인해 많은 재귀함수가 호출되며... 함수들이 메모리 공간을 초과하는게 시간초과보다 더 빨리 발생한 것 같다...

## 두 번째 풀이

- 괜히 visited를 쓰는게 아니구나 하는걸 느꼈다. ㅎㅎ
- 노드 탐색을 할 때는 먼저 visited를 1로 만들고 탐색을 시작하자! ㅎㅎ

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> v[10001];
int dp[10001], mx, visited[10001], n, m, a, b;

int dfs(int here) {
	visited[here] = 1;
	int ret = 1;
	for(int there : v[here]){
		if(visited[there]) continue;
		ret += dfs(there);
	}
	return ret;
}

int main() {
	ios_base::sync_with_stdio(0);
	cin.tie(0);
	cin >> n >> m;
	while (m--) {
     	cin >> a >> b;
	    v[b].push_back(a);
	}
	for (int i = 1; i <= n; i++) {
		memset(visited, 0, sizeof(visited));
		dp[i] = dfs(i);
		mx = max(dp[i], mx);
	}
	for (int i = 1; i <= n; i++) if (mx == dp[i]) cout << i << " ";
	return 0;
}
```
