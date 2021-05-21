---
title: spring cloud practice
date: 2018-07-03
draft: false
tags: ["professional"]
---

# Spring Cloud整体实践

笔者在最近的工作中用到了`Spring Cloud`微服务整体解决方案，没有使用`SC `的全部组件，只用到了网关(Zuul)，注册中心(Eureka)和配置中心(Config server)，现将方案简要说明如下

整体简化版部署图如下

首先api访问会经过load balance，再由负载均衡层将请求转发到网关，在网关层，配置了路由规则，会将api请求转发到对应的服务

![sc](https://i.loli.net/2018/07/03/5b3b83d07d70f.png)



## 网关层

使用的zuul的版本还是1.x，配置方面只要遵循官方的例子就行，没有什么难点，几点可以关注的

1. 将路由规则放到`config`里配置，这样当路由规则变化的时候，只需要更新`config`就行，网关不需要重新发布
2. 1.x的版本没有自带限流器，需要配合第三方的限流器，目前笔者使用的是`spring-cloud-zuul-ratelimit`，使用下来没什么问题，并且[github](https://github.com/marcosbarbero/spring-cloud-zuul-ratelimit)的维护度比较高

## 注册中心层

注册中心没什么说的，使用的是`Eureka`，官方标配

## 配置中心层

配置中心使用的是`config-server`，本地server模式，有一个关键配置是

```yml
spring:
  cloud:
    config:
      server:
        native:
          search-locations: classpath:config/,classpath:config/application.yml
```

在`config/application.yml`里存放了各个应用一些公共的配置，如redis的配置地址，eureka的配置地址，统一的应用端口8080


