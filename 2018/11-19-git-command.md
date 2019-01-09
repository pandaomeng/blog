---
title: git 命令-记录
date: 2018-11-19
tag: [git]
---

# git 命令-记录

1. 强制同步（让本地和远端一致）

   线上服务器出现：

   `您的分支领先 'origin/master' 共 1 个提交。`  

   原因：改动了线上服务器的文件，导致和orgin端的master出现了偏离。

   处理方法：丢弃修改，强制与oring/master 一致

   ```shell
   git fetch --all
   git reset --hard origin/master
   git pull
   ```



​	<!--more-->

2. 回滚

   本地可以通过以下命令强行回到之前的commit节点

   先用 `git log` 找到对应的commit节点，然后

   ```shell
   git reset --hard commint_id
   ```

   使用这条命令后再用 git log 就找不到之后的commit 了，

   这时可以输入下面的命令，查看历史的日志

   ```
   git reflog
   ```



3. 图形化显示

   类似于source tree 的图形化管理，命令

   ```shell
   git log --graph --oneline -20
   ```

   ps: 

   - --oneline：只会显示版本号和提交时的备注信息
   - --graph：以简单的图形方式列出提交记录
   - -n：查看最近n次的提交历史记录，栗子：git log -20，查看20条



4. 强制覆盖远程版本

   如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用`--force`选项。

   ```shell
   git push --force origin
   ```


5. 本地已存在项目，推送到git

   在github上建一个空的项目，然后在本地的项目路径中执行以下命令（或者其他git平台）

   ```shell
   git init
   git add .
   git commit -m "Initial commit"
   git add remote origin git@github.com:pandaomeng/xxx.git #改为你的git地址
   git push -u origin master
   ```

   ps：

   - git push -u：如果当前分支与多个主机存在追踪关系，则可以使用`-u`选项指定一个默认主机，这样后面就可以不加任何参数使用`git push`。



6. 在错误的分支commit（没有push）

   ```shell
   git reset --soft HEAD^
   ```

   ps:

   - HEAD^: 上个commit节点，可以替换为具体的commit id
   - --soft: 只回退了commit的信息，如果还要提交，直接commit即可。文件没有变动。

   接下来只要切换到正确的分支就可以了，他会把未commit的文件改动带过去

   ```shell
   git checkout dev
   ```



7. 修改的文件已被git commit，但想再次修改不再产生新的Commit

   ```shell
   git add sample.txt
   git commit --amend -m"说明"
   ```



8. revert回滚：放弃制定的提交，会产生一个新的commit，历史记录都在

   ```shell
   git revert <commitID>
   ```












