---
title: "프로그래머스 72411 메뉴 리뉴얼"
date: 2022-09-19
tags:
  - cote
---

## 문제

[프로그래머스 72411 메뉴 리뉴얼](https://school.programmers.co.kr/learn/courses/30/lessons/72411)
<br/>

## 풀이 1

너무 긴~~ 내 코드이다. ㅎㅎ<br/>

로직을 간단하게 설명하면,<br/>

1. `orders`에 등장하는 모든 알파벳들을 alpha[26]에 체크한다. (있으면 true)
2. alpha[26]에서 체크한 모든 알파벳들을 가지고, `courese`에 등장하는 모든 조합들을 생성해 `combinations`에 저장한다.
3. 위에서 생성한 `combinations` 에서 answer에 넣을 수 있는 max count(가장 주문이 많이 된) 조합을 탐색한다.
   - `combinations`에서 두 번이상 세트로 주문된 조합들을 찾고,
   - 조합들이 주문된 횟수를 계속 비교하면서 주문된 횟수가 max 인 조합들만 map `candidate`에 삽입한다.
4. 위에서 탐색한 `candidate`(max count 조합들)를 answer에 넣고 정렬한다.

```cpp
int n; // menu nums
bool alpha[26];
vector<string> combinations;

void comb(int choice) {
	vector<char> allMenu;
	for (int i = 0; i < 26; i++) {
		if (alpha[i]) {
			allMenu.push_back(i + 'A');
		}
	}
	vector<bool> temp(n-choice,0);
	for (int i = 0; i < choice; i++) {
		temp.push_back(1);
	}
	do {
		string menuComb = "";
		for (int i = 0; i < n; i++) {
			if (temp[i] == 1) {
				menuComb += allMenu[i]; // 어차피 앞에 원소부터 들어가서 알아서 오름차순
			}
		}
		combinations.push_back(menuComb);
	} while (next_permutation(temp.begin(), temp.end()));
}

vector<string> solution(vector<string> orders, vector<int> course) {
	vector<string> answer;

	// 1. get all menu
	for (string order : orders) {
		for (char o : order) {
			if (alpha[o - 'A']) {
				continue;
			}
			alpha[o - 'A'] = 1;
			n++;
		}
	}

	// 2. get combinations
	for (int c : course) {
		comb(c);
	}

    // 3. get combination candidates
	int s = orders.size();
	map<string, int> candidate[26];
	for (string mc : combinations) {
		int mcnt = 0;
		bool flag = 1;
		for (int i = 0; i < s; i++) {
			for (char m : mc) {
				if (orders[i].find(m) == string::npos) {
					flag = 0;
					break;
				}
				flag = 1;
			}
			if (flag) {
				mcnt++;
			}
		}
		if (mcnt >= 2) {
			if (candidate[mc.size()].size()) {
				auto iter = candidate[mc.size()].begin();
				if ((*iter).second > mcnt) {
					continue;
				}
				if ((*iter).second < mcnt) {
					candidate[mc.size()].clear();
				}
			}
			candidate[mc.size()].insert(make_pair(mc, mcnt));
		}

	}

    // 4. push combinations to answer
	for (int c : course) {
		auto iter = candidate[c].begin();
		for (int i = 0; i < candidate[c].size(); i++) {
			answer.push_back((*iter).first);
			iter++;
		}
	}

	sort(answer.begin(), answer.end());
	return answer;
}

```

## 풀이 2

훨씬 간단한 풀이이다.🤩

```cpp
map<string,int> combi;

void combination(string src, string crs, int depth) {
    if (crs.size() == depth) combi[crs]++;

    else for (int i = 0; i < src.size(); i++)
        combination(src.substr(i+1), crs+src[i], depth);
}

vector<string> solution(vector<string> orders, vector<int> course) {
    vector<string> answer;

    for (string &order : orders)
        sort(order.begin(), order.end());

    for (int crs : course) {
        for (string order : orders)
            combination(order, "", crs);

        int sup = 0;
        for (auto it : combi)
            sup = max(sup, it.second);
        for (auto it : combi)
            if (sup >= 2 && it.second == sup)
                answer.push_back(it.first);
        combi.clear();
    }

    sort(answer.begin(), answer.end());
    return answer;
}
```

### 차이점 : combination 방법

위 풀이에서 나는 `next_permutation`함수를 사용해 조합들을 생성했다. `orders`를 순회하면서 등장하는 모든 알파벳을 체크하고, 등장한 알파벳들로 만들 수 있는 모든 조합들을 생성했다. <br/>

이 풀이에서는 재귀 함수가 사용되었다.

```cpp
void combination(string src, string crs, int depth) {
    if (crs.size() == depth) combi[crs]++;

    else for (int i = 0; i < src.size(); i++)
        combination(src.substr(i+1), crs+src[i], depth);
}
```

```cpp
// solution의 한 부분

for (int crs : course) {
        for (string order : orders)
            combination(order, "", crs);
```

나는 `orders`에 등장한 모든 알파벳을 가지고 조합을 만들었기에 실제 고객이 주문하지 않은 세트도 combinations에 포함되었다. 때문에 3번 `위에서 생성한 combinations 에서 answer에 넣을 수 있는 max count(가장 주문이 많이 된) 조합을 탐색한다.` 라는 로직이 추가되어야만 했다. (해당 조합이 실제 고객이 주문한 세트에 포함되는지 체크...) <br/>

그러나 이 로직은 고객이 주문한 order를 토대로 조합을 생성하기에 나처럼 부가적으로 for문을 돌며 체크할 필요가 없다. 그 뿐만이 아니라 메뉴 주문 횟수 카운트도 그 때 그 때 할 수 있다 ㅎㅎ (나는... 왜 그렇게?...) <br/>

조합을 이렇게 만들수도 있다는걸 배웠다. ^^!

<br/><br/>

`combination`함수에서 고객이 주문한 세트에서 나올 수 있는 조합만을 고려하였고, 조합을 찾음과 동시에 map combi 에서 주문 횟수도 카운트를 했기에 로직이 훨씬 간단해지는 것을 볼 수 있었다.<br/>

<br/>

그래서... 결론은... 조합 함수를 더 야무지게 짜는 방법을... 배웠다...
