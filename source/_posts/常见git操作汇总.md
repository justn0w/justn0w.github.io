---
title: 常见git操作汇总
date: 2019-09-08 16:17:53
tags: git
---

#### 0x01 远程仓库
##### 1. 删除远程仓库中文件
```shell
# 删除文件
git rm --cached "文件名"
git push
# 删除文件夹
git rm -r --cached "文件夹"
git push
```
