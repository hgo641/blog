---
title: "백준 11655 ROT13"
date: 2022-07-11
tags:
  - cote
---

## 문제

'Dummy' 라는 도스게임이 있다. 이 게임에는 뱀이 나와서 기어다니는데, 사과를 먹으면 뱀 길이가 늘어난다. 뱀이 이리저리 기어다니다가 벽 또는 자기자신의 몸과 부딪히면 게임이 끝난다.
<br/>
게임은 NxN 정사각 보드위에서 진행되고, 몇몇 칸에는 사과가 놓여져 있다. 보드의 상하좌우 끝에 벽이 있다. 게임이 시작할때 뱀은 맨위 맨좌측에 위치하고 뱀의 길이는 1 이다. 뱀은 처음에 오른쪽을 향한다.
<br/>
뱀은 매 초마다 이동을 하는데 다음과 같은 규칙을 따른다.
<br/>
먼저 뱀은 몸길이를 늘려 머리를 다음칸에 위치시킨다.
만약 이동한 칸에 사과가 있다면, 그 칸에 있던 사과가 없어지고 꼬리는 움직이지 않는다.
만약 이동한 칸에 사과가 없다면, 몸길이를 줄여서 꼬리가 위치한 칸을 비워준다. 즉, 몸길이는 변하지 않는다.
사과의 위치와 뱀의 이동경로가 주어질 때 이 게임이 몇 초에 끝나는지 계산하라.
<br/>

## 풀이

deque를 이용해 뱀의 body 좌표 정보를 저장했다.<br/>
뱀이 바라보는 방향은 0, 1, 2, 3 총 네가지가 있으며 각각 up, right, down, left를 의미한다. <br/>
초기값을 입력받고 게임을 시작한다.<br/>
뱀의 방향 변화가 일어나는 시간과 현재 시간을 비교하며 2번째 while문을 돌린다.<br/>
뱀이 한 방향으로 가는 만큼 while문을 돌렸다면, 회전 방향에 따라 뱀이 바라보는 방향을 변화시키고 다음 time slice만큼 또 while문을 돌린다.<br/>
가장 바깥의 while문이 break 되었다면,<br/>

1. 뱀이 주어진 방향 변화를 모두 마쳤지만 아직 죽지않았는지(== flag가 0)
2. 뱀이 map의 바운더리에 부딪혔거나, 몸에 부딪혀 죽었는지(== flag가 1)
   판단하고 1번이라면 뱀이 죽을때까지 cur_time을 카운트한다.<br/>
   <br/><br/>
   apple을 먹었다면 apple의 값을 1에서 0으로 바꿔줘야하는 것을 간과해 처음에 실패했었다<br/>
   일어날 경우의 수를 세세하게 짜자!<br/><br/>

```cpp
int N,K,L;
int apple[101][101];
int direction = 1; //0, 1, 2, 3 : up, right, down, left
pair<int, char> rotation[100];
int rotation_cnt = 0;
deque<pair<int, int>> body;
bool flag = 0;
int cur_time = 0;
void change_direction(char c) {
	if (c == 'D') {
		if (direction == 3) {
			direction = 0;
		}
		else direction++;
	}
	else if (c == 'L') {
		if (direction == 0) {
			direction = 3;
		}
		else direction--;
	}
}
bool check_flag(int x, int y, int f) {
	// map idx out of range
	if (f == 1) {
		if (y > N or y < 1) {
			flag = 1;
			return 1;
		}
	}
	if (f == 0) {
		if (x > N or x < 1) {
			flag = 1;
			return 1;
		}
	}

	// body collision
	if (find(body.begin(), body.end(), make_pair(x, y)) != body.end()) {
		flag = 1;
		return 1;
	}
	return 0;
}
int main() {
	//init
	cin >> N >> K;
	int a, b;
	while (K--) {
		cin >> a >> b;
		apple[a][b] = 1;
	}
	cin >> L;
	int t;
	char r;
	for (int i = 0; i < L; i++) {
		cin >> t >> r;
		rotation[i] = make_pair(t, r);
	}
	// game start
	int r_idx = 0;
	body.push_back(make_pair(1,1));

	while (!flag) {
		if (r_idx == L) {
			break;
		}
		while (rotation[r_idx].first != cur_time) {
			pair<int, int> head = body.back();
			int x = head.first;
			int y = head.second;

			// up
			if (direction == 0) {
				x--;
				cur_time++;
				if (check_flag(x, y, 0)) break;
				body.push_back(make_pair(x, y));
			}
			else if (direction == 1) {
				y++;
				cur_time++;
				if (check_flag(x, y, 1)) break;
				body.push_back(make_pair(x, y));
			}
			else if (direction == 2) {
				x++;
				cur_time++;
				if (check_flag(x, y, 0)) break;
				body.push_back(make_pair(x, y));
			}
			else if (direction == 3) {
				y--;
				cur_time++;
				if (check_flag(x, y, 1)) break;
				body.push_back(make_pair(x, y));
			}
			//check apple
			if (apple[x][y] == 1) {
				apple[x][y] = 0;
			}
			else {
				body.pop_front();
			}
		}
		change_direction(rotation[r_idx].second);
		r_idx++;
	}

	// rotation end, but not dead
	if (flag == 0) {
		if (direction == 0) {//up
			pair<int, int> head = body.back();
			int x = head.first;
			int y = head.second;
			while (x > 0) {
				x--;
				cur_time++;
				// body collision
				if (find(body.begin(), body.end(), make_pair(x, y)) != body.end()) {
					break;
				}

				// check apple
				if (apple[x][y] != 1) {
					body.pop_front();
				}
			}
		}
		else if (direction == 1) {//right
			pair<int, int> head = body.back();
			int x = head.first;
			int y = head.second;
			while (y <= N) {
				y++;
				cur_time++;
				// body collision
				if (find(body.begin(), body.end(), make_pair(x, y)) != body.end()) {
					break;
				}
				// check apple
				if (apple[x][y] != 1) {
					body.pop_front();
				}
			}
		}
		else if (direction == 2) {//down
			pair<int, int> head = body.back();
			int x = head.first;
			int y = head.second;
			while (x <= N) {
				x++;
				cur_time++;
				// body collision
				if (find(body.begin(), body.end(), make_pair(x, y)) != body.end()) {
					break;
				}
				// check apple
				if (apple[x][y] != 1) {
					body.pop_front();
				}
			}
		}
		else if (direction == 3) {//left
			pair<int, int> head = body.back();
			int x = head.first;
			int y = head.second;
			while (y > 0) {
				y--;
				cur_time++;
				// body collision
				if (find(body.begin(), body.end(), make_pair(x, y)) != body.end()) {
					break;
				}
				// check apple
				if (apple[x][y] != 1) {
					body.pop_front();
				}
			}
		}
	}
	cout << cur_time;
}
```
