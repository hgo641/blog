---
title: "백준 2869 달팽이는 올라가고 싶다"
date: 2022-07-13
tags:
  - C++
  - cote
series: "코테준비"
---

## 문제

땅 위에 달팽이가 있다. 이 달팽이는 높이가 V미터인 나무 막대를 올라갈 것이다. <br/>

달팽이는 낮에 A미터 올라갈 수 있다. 하지만, 밤에 잠을 자는 동안 B미터 미끄러진다. 또, 정상에 올라간 후에는 미끄러지지 않는다.<br/>

달팽이가 나무 막대를 모두 올라가려면, 며칠이 걸리는지 구하는 프로그램을 작성하시오.<br/>

## 풀이

손으로 하나하나 적어가면서 규칙을 찾았다... <br/>

### 첫 번째 풀이

```c++
double a,b,v;
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	cin >> a >> b >> v;
	double x = v / (a - b);
	x = ceil(x);

	double y = a + x * (a - b); // 낮에 a만큼 먼저 올라가니까 a를 더함

	double z = y - v;

	int sub = z / (a - b);

	int ans = x - sub + 1;
	cout << ans << "\n";

}
```

### 두 번째 풀이

위에서 난리를 치며 풀었지만, 단 한 줄로 간단하게 풀 수 있다. <br/>
`(V - B - 1) / (A - B) + 1`을 하면 원하는 답이 나온다.<br/>
<br/>

- 골에 도달하는 날은 미끄러지지않으므로, V에서 B를 뺀다.
- (V - B)를 하루동안 최종적으로 올라간 만큼인 (A - B)로 나눈다.
- (V - B) / (A - B) 했을 때 딱 나눠 떨어지지 않을 수도 있다.
- 나눠 떨어지지않는다면 올림을 해야 답과 같으므로 마지막에 +1을 해준다.
- 딱 나눠 떨어질 수도 있으므로 미리 (V - B)에 1을 뺀다.
  <br/>
  <br/>

```c++
double a,b,v;
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	cin >> a >> b >> v;

	int ans = (v - b - 1) / (a - b) + 1;

	cout << ans << "\n";
}

```
