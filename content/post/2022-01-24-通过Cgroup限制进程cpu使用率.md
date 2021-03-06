---
title: "通过Cgroup限制进程cpu使用率"
date: 2022-01-24T13:10:07+08:00
draft: true
author: 惨绿少年
type: post
Baidusubmit:
  - 1
categories:
  - Linux运维
  - 虚拟化

---

## 限制方法
1、找到需要限制的进程
```shell
ps -ef | grep mysql
```
![image.png](/yuque/0/2022/png/163067/1642154382841-830055f7-b5bb-4484-86fb-f222d56558e3.png)


2、到cgroup目录创建一个专用目录
```shell
cd /sys/fs/cgroup/cpu/ 
mkdir mysql
cd  mysql 
echo  22112 > cgroup.procs  # 22112 是第一步中找到的进程id
echo  "200000" > cpu.cfs_quota_us  # 这是限制使用率，限制进程可以用到 200%
```
​

3、验证
mysql 进程的cpu使用率最大 200%
![image.png](/yuque/0/2022/png/163067/1642154447099-1e7f8e42-b44c-4363-aec0-3d229b31befa.png)
## 参考文档
[https://www.cnblogs.com/wuchangblog/p/13937715.html](https://www.cnblogs.com/wuchangblog/p/13937715.html)

