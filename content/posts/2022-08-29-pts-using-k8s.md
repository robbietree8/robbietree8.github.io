---
title: performance test using k8s
date: 2022-08-29
draft: false
tags: ["tips"]
---


# 在k8s里做性能测试

## 整体架构

![flow](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/robbietree8/robbietree8.github.io/main/assets/2021-08-29/flow.puml)

其中:

`jmeter-k8s-job`

以`docker-jmeter`为基础镜像，运行`jmx`脚本即可

`influxdb2`

使用的是`2.4.0`版本，按照官方文档部署即可

`grafana`

使用的是`9.1.1`版本，按照官方文档部署即可

两个开源的dashboards见以下。


## 参考

- [docker-jmeter](https://github.com/unseel/docker-jmeter)
- [监控数据库连接池](https://grafana.com/grafana/dashboards/6083-spring-boot-hikaricp-jdbc/)
- [监控jmeter](https://grafana.com/grafana/dashboards/13644-jmeter-load-test-org-md-jmeter-influxdb2-visualizer-influxdb-v2-0-flux/)
