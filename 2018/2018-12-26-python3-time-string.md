---
title: python3 生成时间字符串
date: 2018-12-26
tag: [python3]
---

### 生成对应格式的时间字符串

```python
import time

current_time = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
print(current_time)
```

结果：

```
2018-12-26 14:27:26
```



