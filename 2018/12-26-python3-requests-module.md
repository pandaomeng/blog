---
title: python3 requests 模块发送请求
date: 2018-12-26
tag: [python3]
---

### python3 requests 模块发送请求

1. post请求：

```python
import requests
import json

# 模拟头部信息，和浏览器请求时一致
headers = {
  'Accept': 'application/json, text/plain, */*',
  'Content-Type': 'application/json;charset=UTF-8',
  'Origin': 'xxx',
  'Referer': 'xxx',
  'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36',
  'cookie': 'xxx'
}

# 配置链接和参数，注：这里用的是payload请求方式，formdata的形式有另外写法
url = 'http://www.xxx.com/api/xxx'
data = {
  'id': '10001',
  'token': "xxxxxx"
}
res = requests.post(url, data=json.dumps(data), headers=headers)
print('res: ', res)
```

说明：

- 以上的链接均为虚构的，实际操作替换为真实场景的值
- data=json.dumps(data)，注意要使用这个json函数，才能成功

2. get请求

   ```python
   download_url = 'http://www.xxx.com/api/xxx'
   res = requests.get(download_url, headers=headers)
   ```