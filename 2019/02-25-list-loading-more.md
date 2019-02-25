---
title: 如果优美地实现加载更多
date: 2019-02-25
tag: [miniprogram]
---

## 如果优美地实现加载更多

### 前言

在小程序或者原生app开发的时候，为了更好的体验，需要翻页的列表往往在触底的时候需要显示**加载中**，在没有数据后显示**没有更多了**。

### 分析

1. 一直在底部显示"加载中"，但是在视窗范围内看不到，在触底的时候发起翻页请求（此时可以看到"加载中"），等数据返回后，将"加载中"顶到最下面。
2. 判断count数和list长度相等时隐藏"加载中"，显示"没有更多了"，并且不再请求

<!--more-->

### 实现

假设后端返回的数据结构为：

```json
{
  code: 1001,
  result: {
    count: 37,
    items: [...]
  }
}
```

以小程序为例，我们全局声明变量

```
data: {
  list: [], // 列表
  currentPage: 0, // 当前页
  count: 0, // 列表总数
  pageLoading: false, // 控制页面laoding的变量
},
```

在wxml结构中：

```
<view wx:if="{{ list.length !== count && !pageLoading }}" class="loading-text">
  正在加载<dot>...</dot>
</view>
<view wx:if="{{ list.length === count && !pageLoading }}" class="no-more-text">
  没有更多了~
</view>
```

在js中的逻辑：

```
async onLoad() {
  this.setData({
    pageLoading: true,
  })
  await this.goPage()
  this.setData({
    pageLoading: false,
  })
},
goPage() {
  return new Promise(async (resolve) => {
    const res = await axios.get('/users', {
      page: this.currentPage,
      pageSize: 10,
    })
    if (res.code !== 1001) {
      resolve(false)
    }
    this.setData({
      list: this.data.list.contact(res.result.items || []),
      count: res.result.count,
    })
    resolve()
  })
},
onReachBottom() {
  if (this.data.list.length === this.data.count) return // 没有更多后不再请求
  this.setData({
    currentPage: this.data.currentPage + 1,
  })
  this.goPage() // 执行请求，page参数使用currentPage
},
```

wxss部分，点点点的动效：

```
/* 加载更多 点点点的动效 */
dot {
  display: inline-block; 
  height: 1em;
  line-height: 1;
  text-align: left;
  vertical-align: -.25em;
  overflow: hidden;
}
dot::before {
  display: block;
  content: '...\A..\A.\A';
  white-space: pre-wrap;   /* 也可以是white-space: pre */
  animation: dot 2s infinite step-start both;
}
@keyframes dot {
  25% { transform: translateY(-3em); }
  50% { transform: translateY(-2em); }
  75% { transform: translateY(-1em); }
}
```

css特效参考自张鑫旭的博客：

[CSS3 animation渐进实现点点点等待提示效果](http://www.zhangxinxu.com/wordpress/?p=3411)

