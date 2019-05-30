---
title: 从coding转移git仓库到github
date: 2019-05-30
tag: [git]
---

# 从coding转移git仓库到github

## 步骤

1. 获取源git仓库的地址

   如： git@git.coding.net:pandameng/xxx.git

2. 在github上创建一个空的仓库

   如：git@github.com:pandaomeng/xxx.git

3. 随意找个目录，执行以下命令

   ```
   git clone --bare git@git.coding.net:pandameng/xxx.git # 源仓库地址
   cd xxx.git/
   git push --mirror git@github.com:pandaomeng/xxx.git # 目标地址
   ```

   