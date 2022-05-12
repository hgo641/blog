---
title: "list comprehension"
date: 2022-05-12
tags:
  - python
---

## 중첩 사용

```python
temp = [i*j for i in range(1,4) for j in range(1,5)]

temp = []
for i in range(1,4):
    for j in range(1,5):
        temp.append(i*j)
```

## list comprehension vs zip

```python
target = [0.6, 0.8, 0.5]
actual = [0.726, 0.708, 0.778]

print([target[i]-actual[i] for i in range (len(target))])
print([x-y for x,y in zip(target, actual)])
```
