---
title: python3 unicode 编码
date: 2018-12-26
tag: [python3]
---

### python3 编码

1. unicode码在python里有两种表示方式：

   u'字符串'或者'\u四位十六进制数'。它们是等价的，而且都是str对象。

   栗子：

   ```python
   a = u'中国'
   b = '\u4E2D\u56FD'
   c = '\\u4E2D\\u56FD'
   
   print(type(a), a)
   print(type(b), b)
   print(type(c), c)
   print('a == b:', a == b)
   ```

   结果：

   ```shell
   <class 'str'> 中国
   <class 'str'> 中国
   <class 'str'> \u4E2D\u56FD
   a == b: True
   ```

   <!--more-->

2. `\\u4E2D\\u56FD` 形如这样的字符串转码

   ```python
   c = '\\u4E2D\\u56FD'
   
   print(c)
   print(c.encode().decode('unicode-escape'))
   ```

   结果：

   ```
   \u4E2D\u56FD
   中国
   ```

   ps: encode默认编码方式为utf-8

3. 区别 str 对象 '\u4E2D\u56FD' 和 bytes对象 b'\\u4e2d\\u56fd'

   ```python
   b = u'\u4E2D\u56FD'
   d = b'\u4E2D\u56FD'
   
   print(b.encode('unicode-escape'))
   print(d.decode('unicode-escape'))
   ```

   结果：

   ```
   b'\\u4e2d\\u56fd'
   中国
   ```

   结论：

    - 两者可以通过encode和decode相互转换
    - 他们在书写的时候仅仅单引号前的修饰符有区别

4. 汉字输出成unicode形式（不一定是汉字）

   ```python
   def to_unicode(string):
     ret = ''
     for v in string:
       ret = ret + hex(ord(v)).upper().replace('0X', '\\u')
     return ret
   
   a = '中国'
   print(to_unicode(a))
   ```

   结果：

   ```
   \u4E2D\u56FD
   ```

5. Python3 中的 chr 和 ord 的使用

   ```python
   print(ord('中'))
   print(chr(20013))
   ```

   ```
   20013
   中
   ```
