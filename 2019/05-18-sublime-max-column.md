---
title: sublime 和 vscode 编辑器单行支持的最大长度
date: 2019-05-18
tag: [sublime, vscode]
---

# sublime 和 vscode 编辑器单行支持的最大长度

sublime 当行支持最大长度为 2^14 (即16384)

![](https://images.pandaomeng.com/blog/sublime-column-16384.png)

在输入第16485个字符的时候，sublime失去了色彩。

![](https://images.pandaomeng.com/blog/sublime-max-colmn-16385.png)

而在vscode中，当一行超过1万个字符的时候会显示省略号

![](https://images.pandaomeng.com/blog/vscode-column-10000.png)

