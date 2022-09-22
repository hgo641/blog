---
title: "[c++] 재귀함수로 조합 & 중복조합 구현해보기"
date: 2022-09-22
tags:
  - cote
  - cpp
---

## 재귀함수로 조합 구하기

c++의 경우 조합함수는 보통 `next_permutation` 과 `재귀함수`로 구현하는 것 같다. 단순히 조합 자체를 구하는 거라면`next_permutation`으로 구현하는 것이 재귀함수보다 더 빠르지만, 조합내에서 세부적인 처리가 필요한 문제의 경우 재귀함수를 사용하는게 가독성과 활용성이 좋은 것 같아 오늘 포스팅으로 정리해보려고 한다! 

<br/>

바로 코드부터 봐보자! 아래 코드는 `numbers` {1, 2, 3, 4, 5} 중에서 3개의 숫자를 조합으로 뽑는 코드이다.

```cpp
#include <bits/stdc++.h>

using namespace std;

vector<int> numbers = { 1,2,3,4,5 }; // 뽑을 수 있는 전체 숫자들

void combination(vector<int> combinations, int r, int combIdx, int numIdx) {
	if (r == 0) { // 조합이 완성됨
		for (int c : combinations) {
			cout << c << " ";
		}
		cout << "\n";
		return;
	}

	if (numIdx == numbers.size()) { // r만큼 수를 모으지 못한 combinations
		return;
	}

    // numIdx번째 number를 뽑은 경우
	combinations[combIdx] = numbers[numIdx];
	combination(combinations, r - 1, combIdx + 1, numIdx + 1);

    // numIdx번째 number를 뽑지 않을 경우
	combination(combinations, r, combIdx, numIdx + 1);
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	int r = 3; // 뽑을 개수
	vector<int> combinations(r);
	combination(combinations, r, 0, 0);
}
```

* `numbers`는 뽑을 수 있는 전체 숫자들의 배열이다.
* `combinations` 는 조합들을 일시적으로 담을 배열이다.
  * vector `combinations`는 반드시 뽑을 개수 (r)로 초기화를 해줘야한다.
  * 재귀함수에서 인덱스를 참조하기 때문. 
* `r`은 `numbers`에서 뽑을 개수이다.

> 함수 `combination` 은 조합들을 일시적으로 담을 `combinations` 배열과 r, `combinations`의 인덱스 `combIdx`와 `numbers`의 인덱스 `numIdx`를 인자로 받는다. <br/>
>
> 재귀 함수내에서 인자로 넘겨 받은 numIdx 번 째의 number를 포함한 조합과 포함하지 않은 조합 두 개의 경우를 전부 실행시킨다.<br/>
>
> 재귀함수가 리턴되는 경우는 두 가지가 있다.
>
> * 함수내 인자로 전달된 r은 현재 몇 개의 수를 더 모아야하는지 카운트를 세는 역할을 한다. r이 0일경우, r만큼의 수를 모두 모았다는 뜻이다. 조합에 필요한 수를 모두 모았으면 더 함수를 실행시킬 이유가 없으므로 리턴한다.
> * 재귀함수에서는 numIdx번 째 수를 포함한 조합과 그렇지않은 조합 두 경우를 모두 실행한다. 필연적으로 r만큼의 수를 모두 모으지 못한 조합 후보들이 생긴다. r만큼 뽑는 조합을 만들어야하므로 r만큼 뽑지 못하고 numIdx가 numbers.size()까지 간 함수들은 리턴시킨다.
>
> 모든 조합 후보들이 같은 배열 `combinations`를 사용하지만 해당 함수내에서 `combinations`의 인덱스에 덮어 씌우기 때문에 충돌이 일어날 경우는 없다.





## 재귀함수로 중복 조합 구현하기

바로 코드부터 보자! 조합을 구하는 코드와 거의 틀림그림찾기 정도로 유사한 것을 볼 수 있다.

```cpp
#include <bits/stdc++.h>

using namespace std;

vector<int> numbers = { 1,2,3,4,5 }; // 뽑을 수 있는 전체 숫자들

void combination(vector<int> combinations, int r, int combIdx, int numIdx) {
	if (r == 0) { // 조합이 완성됨
		for (int c : combinations) {
			cout << c << " ";
		}
		cout << "\n";
		return;
	}

	if (numIdx == numbers.size()) { // r만큼 수를 모으지 못한 combinations
		return;
	}

    // numIdx번째 number를 뽑은 경우
	combinations[combIdx] = numbers[numIdx];
	combination(combinations, r - 1, combIdx + 1, numIdx);

    // numIdx번째 number를 뽑지 않을 경우
	combination(combinations, r, combIdx, numIdx + 1);
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	int r = 3; // 뽑을 개수
	vector<int> combinations(r);
	combination(combinations, r, 0, 0);
}
```

<br/>

차이점은 함수`combination`의 아래 부분이다.

```cpp
// [조합] numIdx번째 number를 뽑은 경우
combinations[combIdx] = numbers[numIdx];
combination(combinations, r - 1, combIdx + 1, numIdx + 1);
```

<br/>

```cpp
// [중복조합] numIdx번째 number를 뽑은 경우
combinations[combIdx] = numbers[numIdx];
combination(combinations, r - 1, combIdx + 1, numIdx);
```

중복 조합은 이미 뽑은 숫자여도 다시 뽑을 수 있기 때문에 numIdx번 째 수를 뽑았더라도 numIdx를 1증가하지 않도록 코드를 작성하면 된다.





## 중복 조합 활용 예시 - 프로그래머스 92342 양궁 대회

[프로그래머스 92342 양궁 대회](https://school.programmers.co.kr/learn/courses/30/lessons/92342)

위 문제는 dfs, 비트마스킹등 다양한 방식으로 풀 수 있지만 중복 조합을 활용하여 해결할 수도 있다. 

* 라이언이 쏠 수 있는 모든 경우의 수를 중복 조합으로 구한다.
* 중복 조합들 중 어피치와 점수가 가장 큰 차이로 이긴 조합을 구한다.
* 차이가 같은 조합들이 있다면 그 중 낮은 점수에 더 많이 맞춘 조합을 반환한다.



```cpp
#include <bits/stdc++.h>

using namespace std;
vector<int> numbers = { 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 };
vector<int> prevNums;

int prevDiff = 0; 
int aCnt[11]; // 어피치의 점수를 0점 순으로 카운팅한 배열 (info의 reverse와 동일)

int lionWin(vector<int> lnums) { // 어피치와 라이언의 점수차를 구함
    int lCnt[11] = { 0, };

    for (int c : lnums) {
        lCnt[c]++;
    }
    int ascore = 0;
    int lscore = 0;


    for (int i = 1; i < 11; i++) {
        if (aCnt[i] < lCnt[i]) {
            lscore += i;
            continue;
        }
        if (aCnt[i] == 0 && lCnt[i] == 0) {
            continue;
        }
        ascore += i;
    }
    if (ascore < lscore) {
        return lscore - ascore;
    }
    return 0;
}

bool compareVector(vector<int> cur) { // 더 낮은 점수를 많이 맞춘 조합을 찾음
    int prevCnt[11] = { 0, };
    int curCnt[11] = { 0, };

    for (int p : prevNums) {
        prevCnt[p]++;
    }

    for (int c : cur) {
        curCnt[c]++;
    }


    for (int i = 0; i < 11; i++) { 
        if (prevCnt[i] < curCnt[i]) {
            return 1;
        }
        if (prevCnt[i] > curCnt[i]) {
            return 0;
        }
    }
    return 0;
}


void rcombi(vector<int>temp, int r, int tidx, int nidx) {
    if (r == 0) { // 조합을 완성
        int curDiff = lionWin(temp);
        if (curDiff == 0) { // 라이언의 점수보다 어피치의 점수가 더 높음
            return;
        }
        if (curDiff < prevDiff) { // 이전 중복 조합의 차이값이 더 큼
            return;
        }
        if (prevDiff == curDiff) { // 이전 중복 조합과 차이값이 같음
            if (!compareVector(temp)) { // 더 낮은 점수를 많이 맞춘 조합을 반영
                return;
            }
        }
        prevNums = temp;
        prevDiff = curDiff;
        return;
    }
    if (nidx == numbers.size()) { // n개의 수를 모으지 못한 조합
        return;
    }

    temp[tidx] = numbers[nidx];
    rcombi(temp, r - 1, tidx + 1, nidx);

    rcombi(temp, r, tidx, nidx + 1);

}

vector<int> solution(int n, vector<int> info) {
    int point = 10;
    for (int i : info) {
        aCnt[point--] = i;
    }

    vector<int> answer;
    vector<int> temp(n);

    rcombi(temp, n, 0, 0);

    int cnt[11] = { 0, };
    if (prevNums.size() == 0) { // 라이언이 이길 수 있는 조합이 없음
        answer.push_back(-1);
    }
    else {
        for (int p : prevNums) {
            cnt[p]++;
        }
        for (int i = 10; i >= 0; i--) {
            answer.push_back(cnt[i]);
        }
    }
    return answer;
}
```

> * 재귀함수를 이용해 라이언이 맞출 수 있는 점수의 모든 경우의 수를 구한다.
> * prevDiff와 curDiff를 계속 비교하면 최대 차이값을 가지는 중복 조합을 얻는다.

