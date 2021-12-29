---
title: 微信后端服务TLS设置
date: 2021-12-29
draft: false
tags: ["tips"]
---

# 微信后端服务TLS设置

微信对后台服务器的要求是`TLS 必须支持 1.2 及以上版本`

k8s上如何配置: `nginx-configuration`,`ssl-protocols`里配置成`TLSv1.2 TLSv1.3`

如何验证: 可在[myssl](https://myssl.com/)网站上检查支持的协议版本。


## 参考

- [微信-服务器域名配置](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/network.html#1.%20%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%9F%9F%E5%90%8D%E9%85%8D%E7%BD%AE)
- [k8s-tls](https://kubernetes.github.io/ingress-nginx/user-guide/tls/)
