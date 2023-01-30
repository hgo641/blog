---
title: "백준 15685 드래곤 커브 풀이"
date: 2023-01-30
tags:
  - cote
  - implementation
---

## 문제

[백준 15685 드래곤 커브](https://www.acmicpc.net/problem/15685)<br/>

* 드래곤 커브는 시작점(x, y), 방향(d), 세대(g)를 가지고 있다.
* 0세대 드래곤 커브는 시작점(x, y)에서 방향(d)를 바라보게 그은 길이 1의 선분이다.

* K세대 드래곤 커브는 K-1세대 드래곤 커브를 끝 점을 기준으로 90도 시계 방향 회전 시킨 다음, 그것을 끝 점에 붙인 것이다. (0 < K)



## 풀이 방법

문제에서 나왔듯이 드래곤 커브를 그리는 방법은 두 가지로 나뉘어진다.

* 0세대 : 시작점에서 주어진 방향대로 길이 1만큼의 라인을 그린다.
* 1세대 이상 : 이전 세대에서 그린 커브 라인을 90도 회전해서 끝점끼리 이어붙인다.

<br/>

### 📌 0세대 

0세대 드래곤 커브를 그리는 방법은 매우 간단하다. 시작점을 기준으로 방향대로 1만큼 라인을 그리면 된다.

```cpp
int dx[4] = { 1, 0, -1, 0 };
int dy[4] = { 0, -1, 0, 1 };

pair<int, int> drawLine(int x, int y, int d) { // (x, y)에서 d방향으로 1만큼 라인을 그림 
	board[x][y] = 1; // board에 드래곤 커브의 라인 표시
	int cx = x + dx[d];
	int cy = y + dy[d];
	board[cx][cy] = 1;
	return { cx, cy };
}
```



### 📌 1세대 이상

* 우선 K세대 드래곤 커브를 그리기 위해서는 K-1세대 드래곤 커브가 어떻게 생겼는지를 알아야 한다. (K-1세대 커브의 모든 루트를 알아야 함.)

* K가 1이상일 때, K세대 드래곤 커브는 K-1세대 드래곤 커브의 끝점과 K-1세대 드래곤 커브를 90도 회전한 것의 끝점을 서로 이어붙인다. 즉, K세대에서 새로 그려지는 라인은 **K-1세대의 끝점에서 시작**하고, 회전한 K-1세대 커브는 **끝점에서 부터 그려지게 된다.**<br/>

```cpp
int size = curv.route.size(); // curv.route는 K-1세대 드래곤 커브의 루트
pair<int, int> lineStartPoint = curv.route[size - 1]; // K-1세대 드래곤 커브의 끝점

for (int i = size - 1; i > 0; i--) { // 회전한 K-1세대 커브는 끝점에서 커브의 끝점에서부터 그려진다.
    int nextDirection = getNextDirection(curv.route[i], curv.route[i - 1]); //90도 회전한 방향
    pair<int, int> lineEndPoint = drawLine(lineStartPoint.first, lineStartPoint.second, nextDirection); // 길이 1만큼 라인을 그림
    curv.route.push_back(lineEndPoint);
    lineStartPoint = lineEndPoint; // 끝점에서 부터 계속 이어 그림
```



## 전체 코드

```cpp
#include <bits/stdc++.h>

using namespace std;

struct Curv {
	int x;
	int y;
	int d;
	int g;
	pair<int, int> endPoint;
	vector<pair<int, int>> route;
};

int n;
bool board[101][101];

int dx[4] = { 1, 0, -1, 0 };
int dy[4] = { 0, -1, 0, 1 };

int getNextDirection(pair<int, int> src, pair<int, int> dest) {
	if (src.second < dest.second) { // +y
		return 2;
	}
	if (src.first > dest.first) { // -x
		return 1;
	}
	if (src.second > dest.second) { // -y
		return 0;
	}
	if (src.first < dest.first) { // +x
		return 3;
	}
}

pair<int, int> drawLine(int x, int y, int d) { // (x, y)에서 d방향으로 1만큼 라인을 그림 
	board[x][y] = 1;
	int cx = x + dx[d];
	int cy = y + dy[d];
	board[cx][cy] = 1;
	return { cx, cy };
}

void drawCurv(int depth, Curv curv) { // 드래곤 커브 curv의 모든 라인을 그림 
	if (depth > curv.g) {
		return;
	}

	if (depth == 0) {
		curv.route.push_back({ curv.x, curv.y });
		pair<int, int> lineEndPoint = drawLine(curv.x, curv.y, curv.d);
		curv.route.push_back(lineEndPoint);
	}

	else {
		int size = curv.route.size();
		pair<int, int> lineStartPoint = curv.route[size - 1];
		for (int i = size - 1; i > 0; i--) {
			int nextDirection = getNextDirection(curv.route[i], curv.route[i - 1]);
			pair<int, int> lineEndPoint = drawLine(lineStartPoint.first, lineStartPoint.second, nextDirection);
			curv.route.push_back(lineEndPoint);
			lineStartPoint = lineEndPoint;
		}
	}

	drawCurv(depth + 1, curv);
}

int countSquare() {
	int cnt = 0;
	for (int i = 0; i < 100; i++) {
		for (int j = 0; j < 100; j++) {
			if (board[i][j] && board[i + 1][j] && board[i][j + 1] && board[i + 1][j + 1]) {
				cnt++;
			}
		}
	}
	return cnt;
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL); cout.tie(NULL); 
	cin >> n;
	int x, y, d, g;
	for (int i = 0; i < n; i++) {
		cin >> x >> y >> d >> g;
		Curv curv = { x, y, d, g };
		drawCurv(0, curv);
	}
	cout << countSquare();
}
```



## 풀이 방법 2

드래곤 커브 루트의 규칙성을 찾아 해결하는 방법도 있었다.<br/>

* 0번 방향으로 시작하는 드래곤 커브가 있을 때, 각 세대마다 새롭게 그려지는 커브의 방향 변화를 관찰해보자. ( 0 : 우, 1 : 상, 2 : 좌, 3 : 하 )

```
1세대[1개] : 0
2세대[1개] : 1
3세대[2개] : 2 1
4세대[4개] : 2 3 2 1
5세대[8개] : 2 3 0 3 2 3 2 1
```

<br/>

1. 2세대부터 2의 배수씩 늘어나는 것을 볼 수 있다. (당연함. K-1세대 커브를 이어 붙이기 때문.)

2. 2세대부터 ( K-1세대의 역순 + 1 % 4 ) + ( K-1세대 )  씩 늘어나는 것을 볼 수 있다. 

```
2세대[1개] : 1
3세대[2개] : 2 (1)
4세대[4개] : 2 3 (2 1)
5세대[8개] : 2 3 0 3 (2 3 2 1)
```

* 괄호안의 방향은 K-1세대 방향과 동일하다.

<br/>

```
3세대[2개] : 2 1
4세대[4개] : (2 3) (2 1)
```

* 앞의 2, 3은 이전 세대인 3세대의 역순 + 1 % 4와 동일하다.
* `3세대 (2 1) 의 역순` -> `(1 2)의 + 1 % 4`는 `(2 3)`이 된다. 

<br/>

왜? <br/>

* \+1 % 4를 하는 이유 : 90도 회전을 해야하기 때문

> (0 -> 1), (1 -> 2), (2 -> 3), (3 -> 0) 

* 역순인 이유 : 끝점과 끝점끼리 이어지기 때문

> 4세대[4개] : 2 3 2 1를 예시로 둘 때 (2 1)은 기존에 그려진 K-1세대 커브로 시작점부터 그려진다. 그러나  (2 3)은 2가 시작점, 3이 끝점으로 끝점부터 그려지므로 K-1세대 커브를 역순으로 나열한다.



## 전체 코드

```cpp
#include<bits/stdc++.h>
using namespace std;
const int dy[] = { 0, -1, 0, 1 };
const int dx[] = { 1, 0, -1, 0 };
vector<int> dragon[4][11];
int cnt, n, x, y, d, g, a[101][101];
void go(int x, int y, int d, int g) {
	int _x = x;
	int _y = y;
	a[_x][_y] = 1;
	for (int i = 0; i <= g; i++) {
		for (int dir : dragon[d][i]) {
			_x += dx[dir];
			_y += dy[dir];
			a[_x][_y] = 1;
		}
	}
	return;
}
void makeDragon() {
	for (int i = 0; i < 4; i++) {
		dragon[i][0].push_back(i);
		dragon[i][1].push_back((i + 1) % 4);
		for (int j = 2; j <= 10; j++) {
			int _n = dragon[i][j - 1].size();
			for (int k = _n - 1; k >= 0; k--) {
				dragon[i][j].push_back((dragon[i][j - 1][k] + 1) % 4); // 역순 90도 회전
			}
			for (int k = 0; k < _n; k++) {
				dragon[i][j].push_back(dragon[i][j - 1][k]); // K-1세대 커브
			}
		}
	}
	return;
}
int main() {
	cin >> n;
	makeDragon();
	for (int i = 0; i < n; i++) {
		cin >> x >> y >> d >> g;
		go(x, y, d, g);
	}
	for (int i = 0; i <= 100; i++) {
		for (int j = 0; j <= 100; j++) {
			if (a[i][j] && a[i + 1][j] && a[i + 1][j + 1] && a[i][j + 1])cnt++;
		}
	}
	cout << cnt << "\n";
	return 0;
}
```



## 이런 문제... 어떻게 풀어야 할까?

드래곤 커브 문제의 핵심은 K세대 드래곤 커브를 어떻게 그리는가였다. (0 < K) 드래곤 커브는 `방향`과 `세대`에 따라 모양이 달라진다. 

<br/>

첫 번째 풀이의 경우, 직관적으로 커브를 그리는 알고리즘을 작성했다. 이 또한 문제를 해결할 수 있지만 더 빠른 시간안에 풀 수 있는 두 번째 풀이가 존재한다. 두 번째 풀이는 드래곤 커브의 모양에 영향을 끼치는 `방향`과 `세대`를 가지고 커브가 가지게 되는 방향의 규칙성을 찾아 풀었다.<br/>

조건이 두 개나 있어서 (`방향`과 `세대`) 규칙성을 찾기 어려울 때는 조건중 하나를 기준으로 상태를 나열해보는 것도 좋은 방법인 것 같다.<br/>

두 번째 풀이의 경우, `방향`을 기준으로 상태를 나열하다가 규칙성을 찾아냈다.

```
# 각 세대에서 나열되는 방향 (처음 방향이 0이었을 때)
1세대[1개] : 0
2세대[1개] : 1
3세대[2개] : 2 1
4세대[4개] : 2 3 2 1
5세대[8개] : 2 3 0 3 2 3 2 1
```

물론 드래곤 커브의 성질을 잘 이해했다면 나열해보지 않고서도 규칙성을 떠올릴 수 있겠지만... 나는 바로 못떠올릴 것 같기 때문에...^^ 모르겠으면 나열해보면서 찾아야겠다! 😄



## 참고

* https://m.blog.naver.com/PostView.naver?blogId=jhc9639&logNo=221755725380&referrerCode=0&searchKeyword=15685
