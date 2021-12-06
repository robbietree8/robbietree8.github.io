---
title: swagger behind proxy
date: 2021-12-06
draft: false
tags: ["craftsman"]
---

# swagger behind proxy

## 出发点

在某些情况下，我们的后端服务是隐藏在网关后面，网关通过路径路由到具体的后端服务，比如`/abc/` -> `http://backend:8080`

这会导致访问`swagger`端点的时候，路径带不过去，进而通过`swagger`访问不到后端


## 如何解决

1. 配置`ui`地址

```
springdoc.swagger-ui.config-url=${contextPath}/v3/api-docs/swagger-config
springdoc.swagger-ui.url=${contextPath}/v3/api-docs
```

2. 增加`server`配置，详情见参考#1

```
server.forward-headers-strategy=framework
```


## 参考

- [offical springdoc](https://springdoc.org/faq.html#how-can-i-deploy-springdoc-openapi-ui-behind-a-reverse-proxy)
