---

title: 实现ssh 自动登录
date: 2018-05-30
tag: [linux]
---

（expect 可能需要安装）

```
#!/usr/bin/expect

set timeout 60
set host ***.**.**.***
set name root
set password 123456

spawn ssh $host -l $name
expect {
      "(yes/no)?" {
           send "yes\n"
           expect "password:"
           send   "$password\n"
      }
      "password:"  {
          send "$password\n"
      }
}
interact
```

**PS: **

- spawn命令：spawn是进入expect环境后才可以执行的expect内部命令。
- interact：执行完成后保持交互状态，把控制权交给控制台，这个时候就可以手工操作了。

<!--more-->

#### 实现git自动pull（http）

```
#!/usr/bin/expect -f
set uname pandaomeng
set password 123456
set timeout 3

cd /home/workspaces/vue-demo #项目路径
spawn git pull origin dev
expect "*Username*" {send "$uname\r"; exp_continue}
expect "*Password*" {send "$password\r"}
interact
```









##### 



