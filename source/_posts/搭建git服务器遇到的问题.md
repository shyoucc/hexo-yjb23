---
title: 搭建git服务器遇到的问题
date: 2017-06-06 21:18:41
tags:
- linux
- git
---
搭建git服务器，本地push的时候报错:

```shell
remote: fatal: Unable to create temporary file '/var/git/gitblog.git/./objects/pack/tmp_pack_XXXXXX': ????
error: unpack failed: index-pack abnormal exit
```
开始以为是git的hooks钩子出现问题，查了一圈钩子，运行没错，才想到是文件权限，一看确实，因为没有其他用户，开放所有权限:

```
chmod 777 /
```
好了

