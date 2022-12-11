---
title: docker安装MySQL5.7
sticky: 999
categories: docker
tags:
  - docker
  - MySQL
  - MySQL5.7
abbrlink: 8dcdc3f
date: 2022-12-11 19:48:53
description: 教你怎么在docker中快速安装MySQL5.7。妈妈再也不用担心我不会安装MySQL了！
---
在docker中安装MySQL5.7比较简单，直接执行以下命令就可以了：
```shell
docker run -p 3306:3306 --name mysql5.7 \
--restart=always \
-v /root/docker/mysql/conf:/etc/mysql/mysql.conf.d \
-v /root/docker/mysql/logs:/var/log/ \
-v /root/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql:5.7

```
**注意：**“/root/docker/mysql”是我们主机的目录，自己提前用mkdir命令把相关目录建好，例如：
```shell
mkdir -p /root/docker/mysql/conf
```

**命令中各个参数的意义：**
“-p 3306:3306”：将容器内部的3306端口映射到机器上，使得MySQL可以在容器外被连接。
“--name mysql5.7”：这个容器的名字叫mysql5.7。
“--restart=always”：在容器退出时总是重启容器。意思就是docker重启后会自动启动这个容器，不需要再手动执行docker start命令
“-v /root/docker/mysql/conf:/etc/mysql/mysql.conf.d”：将容器内的“/etc/mysql/mysql.conf.d”目录挂载到我们主机的“/root/docker/mysql/conf”目录
