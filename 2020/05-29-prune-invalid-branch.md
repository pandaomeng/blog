---
title: 清除无效的分支
date: 2020-05-29
tag: [笔记, git]
---

# 清除无效的分支

### 清除本地中无效的远程追踪分支

通过下列的命令可以删除 `本地中无效的远程追踪分支`

即：远端已经删除过了，本地还存在 `remotes/origin/fix/foo`，则可通过这个命令清理

```
git remote prune origin
```

该命令不会删除本地的分支

### 清除本地的分支

通过以下命令我们可以找到所有带有 fix 或者 feat 的分支

```
git branch | grep -E 'fix|feat'
```

如果我们想删除这些分支，只需执行以下命令

```
git branch | grep -E 'fix|feat' | xargs git branch -D
```

如果你只想保留本地中某些分支，我们可以通过 grep -v 反选的功能来查找

```
git branch | grep -vE 'develop|module|release'
```

同理，我们再添加删除分支的管道 xargs git branch -D

```
git branch | grep -vE 'develop|module|release' | xargs git branch -D
```

执行完以上命令，我们本地就得到了只包含有 'develop|module|release' 的分支了



### **千万注意，如果要执行删除分支的命令，一定要确保不要到删除自己正在开发的分支！**



