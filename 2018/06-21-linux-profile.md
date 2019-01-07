---
title: linux之profile、bash_profile、bashrc
date: 2018-06-21
tag: [linux]
---

## profile、bash_profile、bashrc文件

1. `profile` (/etc/profile`)

   用于设置系统级的环境变量和启动程序，在这个文件下配置会对**所有用户**生效。当用户登录（`login`）时，文件会被执行。

2. `bashrc` (/etc/bashrc)

   这个文件用于配置函数或别名。`bashrc`文件有两种级别：系统级的位于`/etc/bashrc`、用户级的`~/.bashrc`，两者分别会对所有用户和当前用户生效。 

3. `bash_profile` (~/.bash_profile)

   `bash_profile`只有单一用户有效，文件存储位于`~/.bash_profile`，该文件是一个用户级的设置，可以理解为某一个用户的`profile`目录下。这个文件同样也可以用于配置环境变量和启动程序，但只针对单个用户有效。

   和`profile`文件类似，`bash_profile`也会在用户登录（`login`）时生效，也可以用于设置环境变理。但与`profile`不同，`bash_profile`只会对当前用户生效。

    <!--more-->

## 设置alias别名

1. 编辑.bashrc文件

   ```
   vim ~/.bashrc
   ```

2. 设置别名

   ```
   alias ll='ls -l'
   ```

   注意：等号两边不能有空格

   保存退出

3. 使配置文件生效

   ```
   source ~/.bashrc
   ```

4. 测试设置的命令是否生效





