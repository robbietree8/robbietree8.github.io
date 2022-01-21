---
title: 对COLA的理解
date: 2022-01-21
draft: false
tags: ["arch"]
---

一些问题

1. 外部输入是怎么转换为内部的模型的
   1. 外部输入定义在client模块dto包
2. 对外的API是如何设计的，包括code, message的设计
   1. 关键字段: success, errCode, errMessage
3. 外部服务是如何隔离的
   1. domain层定义接口，infrastructure层负责实现
4. 它是怎么分层、分包的
   1. 先看domain，再看app
   2. 先领域分包，后功能分包





包的定义(摘自https://blog.csdn.net/significantfrank/article/details/110934799)

| 层次 | 包名 | 功能 |
|---|---|---|
| Adapter层 |	web |	处理页面请求的Controller	|
| Adapter层 |	wireless |	处理无线端的适配	|
| Adapter层 |	wap |	处理wap端的适配	|
| App层 |	executor |	处理request，包括command和query	|
| App层 |	consumer |	处理外部message	|
| App层 |	scheduler |	处理定时任务	|
| Domain层 |	model |	领域模型	|
| Domain层 |	ability |	领域能力，包括DomainService	|
| Domain层 |	gateway |	领域网关，解耦利器	|
| Infra层 |	gatewayimpl |	网关实现	|
| Infra层 |	mapper |	ibatis数据库映射	|
| Infra层 |	config |	配置信息	|
| Client | SDK |	api	服务对外透出的API	|
| Client | SDK |	dto	服务对外的DTO	|

