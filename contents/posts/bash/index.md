---
title: "bash: vi: command not found"
date: 2022-06-09
tags:
  - bash
---

bash에서 vi/vim하려는데 "bash: vi: command not found" vi명령어 없다고 할 때

1차 시도

```bash
sudo apt-get install vim
```

<br/>
결과 : E: Unable to locate package vim

<br/><br/>
2차 시도

```bash
apt-get update && apt-get install apt-file -y && apt-file update && apt-get install vim -y
```

<br/>
apt-get을 업데이트하고 했더니 잘된다!
