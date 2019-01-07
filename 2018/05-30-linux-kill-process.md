---
title: linux 之杀死进程
date: 2018-05-30
tag: [linux]
---

最近在公司经常要重启某一个项目， 我一般的做法是先

```
ps -ef | grep 项目名
```

然后复制该进程的pid，再执行

```
kill -9 pid
```

再重新执行启动命令。虽然只有几个简单的步骤，但是执行这种重复的操作多了，浪费的时间也就随之增加了。

<!--more-->

#### 查找某程序的pid并保存在变量中

```
PID=`ps -ef | grep node| grep -v grep | awk -F ' ' '{print $2}'`
```

实例中我要找一个名字中带有node的进程，并把它的pid赋值给PID。

**PS：**

- grep node: 筛选出名字中带有node的结果
- grep -v grep：-v 选项表示反选，可以防止这条命令也出现在结果中
- awk：将结果分成数组的形式，通过{print $2}来获取第二个元素



#### kill掉该进程

```
if [ ! -z "$PID" ]; then
    echo $PID
    echo "Node already exists!"
    kill -9 $PID
fi
```



#### 接下来

你就可以为所欲为啦(～￣▽￣)～





