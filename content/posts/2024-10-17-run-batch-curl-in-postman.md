---
title: 在 postman 里批量运行 curl 命令
date: 2024-10-17
draft: false
tags: ["craftsman"]
---

# 在 postman 里批量运行 curl 命令

## Step1

打开Chrome Dev Tools，右键点击想要执行的链接，选择`Copy as cURL`。


## Step2

在 Postman 中，新建 Collection，然后点击`Import`，将之前复制的 cURL 命令粘贴到文本框中，同时Collection选择新建的Collection。


## Step3

新建 Variables，绑定到新建的 collection 上。


## Step4

点击Run Collection，选择执行次数(Iterations)和延迟(Delay)。
Data file上传 csv 文件，csv 文件里一列对应一个变量，变量名在第一行

## 参考

__kimi的回答__

![1.jpg](https://s2.loli.net/2024/10/17/R6FewNWUXhsQoSd.jpg)

__google的回答__
![2.jpg](https://s2.loli.net/2024/10/17/w8RBjKUGEYpXzSo.jpg)