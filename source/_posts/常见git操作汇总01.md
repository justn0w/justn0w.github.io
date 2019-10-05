---
title: 常见git操作汇总01
date: 2019-10-01 11:07:47
tags: git
---

使用ssh建立git远程仓库和本地仓库连接
<!---more-->
https://blog.csdn.net/chenxi_li/article/details/93380649 (详细步骤)
https://blog.csdn.net/qwaszx523/article/details/79072276 (查询git config信息)
#### 步骤一：设置git提交时的用户名和邮箱

注：使用github账户的用户名和邮箱
##### 1. 全局设置
```shell
git config --global user.name 用户名
git config --global user.email 邮箱

如：
git config --global user.name justnow
git config --global user.email 123@123.com
``` 
可以查看一下此时配置的信息是否正确：

```shell
git config --global --list
```

#### 步骤二：创建SSH key

在bash中输入以下内容
```shell
ssh-keygen -t rsa -C "youremail@example.com"
```

