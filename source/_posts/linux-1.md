---
title: linux-后台执行
date: 2021-10-22 14:24:32
tags: linux
categories: linux
---
### 后台运行
<!--more-->
```
nohup [command] &
```
该命令启动的后台进程可能会被杀掉，解决办法是写在一个脚本里面，示例如下：
```
#!/bin/bash

BUILD_ID=dontKillMe

nohup [command] &
```
执行该脚本