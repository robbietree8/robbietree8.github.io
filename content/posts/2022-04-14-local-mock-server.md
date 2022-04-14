---
title: Running mock server in local
date: 2022-04-14
draft: false
tags: ["craftsman"]
---

# Running local mock server

## Motivation

我的服务依赖一个外部服务，当我想要测试我的服务的时候，我希望可以不去启动这个外部服务。

## Solution

`moco`提供了一种能力，让你可以通过`json`描述请求和响应，只需要执行`standalone`的程序即可启动`mock`服务。
具体如下:

首先从官网下载`standalone`的`jar`包，在同目录下，编写`json`文件，比如

```json
[
  {
    "request": {
      "uri": "/foo/bar"
    },
    "response": {
      "headers": {
        "content-type": "application/json"
      },
      "text": "{\"foo\": \"bar\"}"
    }
  }
]
```

打开命令行，执行`java -jar moco-runner-1.3.0-standalone.jar http -p 12306 -c foo.json`

这时候你就可以通过`http`协议访问对应的端口了

```
❯ curl http://localhost:12306/foo/bar
{"foo": "bar"}%
```

## 参考

- [moco](https://github.com/dreamhead/moco)
