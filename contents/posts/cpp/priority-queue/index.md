---
title: "[c++] Priority Queue로 최소힙과 최대힙 구현하기"
date: 2022-09-06
tags:
  - cote
  - cpp
---

## 최대 힙

## 사건의 발단...

[백준 11279 최대힙](https://www.acmicpc.net/problem/11279)

코린이인 나는 위 문제를 풀기 위해 아래와 같이 코드를 짰다.

### 풀이1

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, v, tail, par_idx, lidx, ridx, temp;

void heap_insert(int* arr) {
	arr[tail] = v;
	par_idx = tail / 2;
	int cur = tail;
	while (par_idx >= 1) {
		if (arr[par_idx] < v) {
			temp = arr[par_idx];
			arr[par_idx] = v;
			arr[cur] = temp;
			cur = par_idx;
			par_idx = par_idx / 2;
		}
		else break;
	}
}

void heap_delete(int* arr) {
	if (tail == 1) { // empty
		cout << "0\n";
		return;
	}
	cout << arr[1] << "\n";
	tail--;
	arr[1] = arr[tail];
	arr[tail] = 0;
	// down
	int cur_idx = 1;

	while (true) {
		lidx = cur_idx * 2;
		ridx = cur_idx * 2 + 1;

		if (lidx > tail && ridx > tail) {
			break;
		}

		int Left = arr[lidx];
		int Right = arr[ridx];
		int max_idx = Left > Right ? lidx : ridx;
		if (arr[cur_idx] < max(Left, Right)) {
			temp = arr[cur_idx];
			arr[cur_idx] = arr[max_idx];
			arr[max_idx] = temp;
			cur_idx = max_idx;
		}
		else break;
	}
}

int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	tail = 1;
	int* arr = new int[n + 2]{ 0, };
	while (n--) {
		cin >> v;

		if (v) { // insert
			heap_insert(arr);
			tail++;
		}
		else { // delete
			heap_delete(arr);
		}
	}
}
```

> 최대힙의 로직을 정직하게 코드로 작성했다...

그런데, 다른 사람들의 풀이를 보니 우선순위 큐(Priority Queue)를 사용하면 쉽게 풀 수 있다는걸 알게 되었다.

<br/>

### 우선순위 큐(Priority Queue)

c++의 `#include<queue>`헤더를 사용해서 만들 수 있다. Heap으로 구현되었다고 하며, 따라서 O(logN)만에 연산이 가능하다.<br/>

- 자동으로 정렬을 해주는데 디폴트로 내림차순 정렬을 수행한다.
- 물론 정렬 방식은 우선순위 큐를 선언할 때 임의로 바꿀 수 있다.
  - 형식) priority_queue<자료형, 컨테이너, 비교함수> 변수명
  - ex) `priority_queue<int, vector<int>, cmp>> v`

### 풀이2 - Priority Queue 사용

- c++의 우선순위 큐는 내림차순 정렬을 디폴트로 지원하고 있으므로 최대 힙을 쉽게 구현할 수 있다.

<br/>

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, v;

int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	priority_queue<int> pq;
	while(n--) {
		cin >> v;

		if (v) { // insert
			pq.push(v);
		}
		else { // delete
			if (pq.empty()) {
				cout << "0" << "\n";
				continue;
			}
			cout << pq.top() << "\n";
			pq.pop();
		}
	}

}
```

## 최소 힙

[백준 1927 최소힙](https://www.acmicpc.net/problem/1927)<br/>

최대 힙을 구현해봤으니 최소 힙도 구현해보자! 이것도 priority queue를 쓰면 쉽게 구현할 수 있을 것이다.

### 비교함수 cmp

그러나 priority_queue의 디폴트는 내림차순 정렬이므로 비교함수를 임의로 바꿔줘야한다. 앞서 말했듯이 비교함수는 큐를 생성할 때 넣어서 바꿔주면 된다.<br/> 그럼 비교함수는 그냥 메서드 하나를 넣어주면 될까? priority queue의 생성자에 들어가는 매개 변수는 다음과 같다.

```
template <class Type, class Container= vector <Type>, class Compare= less <typename Container ::value_type>>
class priority_queue
```

앞에 Type은 int와 같이 원소의 형식이고 뒤에 컨테이너는 말 그대로 요소들이 담길 컨테이너이다. 우리가 봐야할 것은 마지막 less인데 이게 비교함수를 의미한다.

<br/>

그런데... less가 뭐임?<br/>

cpp문서를 살펴보면 less는 struct 형태이며 안에 멤버 함수로는 operator()가 있는데 형태는 아래와 같다고 한다.

```cpp
constexpr bool operator()(const T &lhs, const T &rhs) const
{
    return lhs < rhs; // assumes that the implementation uses a flat address space
}
```

이걸 사용해서 내림차순 정렬을 구현해보자!

```cpp
struct cmp {
	constexpr bool operator()(const int &a, const int &b) const {
		return a > b;
	}
};
```

- 매개 변수 순서대로 a가 먼저 들어왔고 b가 뒤에 들어왔다고 생각하면 편하다.
- return값이 양수이면 순서를 바꾸고 음수이면 순서를 바꾸지 않는다.
- 나는 오름차순 정렬을 해야하므로 앞에 서있는 a가 b보다 크다면 순서를 바꿔줘야한다.

<br/><br/>

struct less에 대해 자세한건 아래 링크에 나와있다.<br/>

https://en.cppreference.com/w/cpp/utility/functional/less

### 풀이 - Priority Queue 사용

앞서 만든 비교함수를 적용한 최소 힙 구현 풀이이다.

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, v;

struct cmp {
	constexpr bool operator()(const int &a, const int &b) const {
		return a > b;
	}
};
int main() {
	ios_base::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	cin >> n;
	priority_queue<int,vector<int>,cmp> pq;
	while(n--) {
		cin >> v;

		if (v) { // insert
			pq.push(v);
		}
		else { // delete
			if (pq.empty()) {
				cout << "0" << "\n";
				continue;
			}
			cout << pq.top() << "\n";
			pq.pop();
		}
	}
}
```
