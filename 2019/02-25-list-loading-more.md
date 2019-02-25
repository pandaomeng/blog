---
title: 如果优美地实现加载更多
date: 2019-01-11
tag: [nginx]
---

## 如果优美地实现加载更多

### 前言

在小程序或者原生app开发的时候，为了更好的体验，需要翻页的列表往往在触底的时候需要显示**加载中**，在没有数据后显示**没有更多了**。

### 分析

1. 在触底的时候发起翻页请求，此时显示"加载中"，等数据返回后，隐藏"加载中"，由 moreLoading 控制
2. 判断count数和list数相等时显示"没有更多了"，并且不再请求

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
	moreLoading: false, // 是否正在加载更多
	pageLoading: false, // 控制页面整体的loading的变量
}
```

在wxml结构中：

```
<view wx:if="{{ moreLoading }}" class="loading-text">
  正在加载...
</view>
<view wx:elif="{{ (list.length === count) && !pageLoading }}" class="no-more-text">
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
	return new Promise(async resolve => {
	  	const res = await axios.get('/users', {
            page: this.currentPage,
            pageSize: 10,
		})
		if (res.code !== 1001) {
            resolve(false)
		}
		this.setData({
			list: list.contact(res.result.items || []),
			count: res.result.count,
		})
        resolve()
	})
},
async onReachBottom() {
	if (list.length === this.data.count) return // 没有更多后不再请求
    this.setData({
    	currentPage: currentPage + 1,
    	moreLoading: true,
    })
    await this.goPage() // 执行请求，page参数使用currentPage
    this.setData({
   		moreLoading: true,
    })
},
```

