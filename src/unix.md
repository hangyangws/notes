## yum

`yum list installed`：查看已经安装的包  
`yum -y install packageName`：安装包「-y 表示所有的对话都选择同意」
`yum -y remove packageName`：卸载包「-y 表示所有的对话都选择同意」

## centOS 安装 php 环境

### 安装 Apache

1. `yum install httpd` 安装 Apache
1. 浏览器输入 IP 地址就可以访问了

### 安装 php

1. `yum install php` 安装 php
1. `service httpd restart` 重启 Apache
1. 浏览器输入 IP 地址就能看到 phpInfo 了

### lsof

[lsof 一切皆文件](http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/lsof.html)

### 查看端口占用

`lsof -i:[port]`  
lsof「list open files」

### 并且杀掉进程

kill PID
