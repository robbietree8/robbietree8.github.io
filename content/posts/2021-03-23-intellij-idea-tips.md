---
title: intellij idea tips
date: 2021-03-23
draft: false
---

# Intellij IDEA tips

IDEA提供了很多提效的工具，比如很多人喜欢的重构菜单，今天我们来了解下其他的工具。


## Live Template

这个代码模版工具可以让你在IDE里配置常用的代码模版。

### 内置

写个`main`函数

![lt-main](https://i.loli.net/2021/03/23/iOYGq7Qotr52W6L.gif)


写个`for`循环

![lt-fori](https://i.loli.net/2021/03/23/QK12b6gpEWmhVyq.gif)


### 自定义

写后端的同学经常需要写一些控制器，`GetMapping`, `PostMapping`啥的。

```java
@GetMapping("/test1")
@ResponseBody
public ResponseEntity<String> test1() {
    return ResponseEntity.ok("");
}
```


我们可以利用`Live Templates`少写一点代码。

配置如下
![-w982](https://i.loli.net/2021/03/23/s4NwJBglMRtfp5A.jpg)


```
@org.springframework.web.bind.annotation.GetMapping("$EXPR1$")
@org.springframework.web.bind.annotation.ResponseBody
public ResponseEntity<$EXPR2$> $EXPR3$() {
    $END$
    return ResponseEntity.ok($EXPR4$);
}
```

试试看效果

![lt-getmapping-2](https://i.loli.net/2021/03/23/QuSCvJgTiDKsoG6.gif)


## Postfix Template

后缀代码补全

### 内置

> 打印到标准输出

![pt-systemout](https://i.loli.net/2021/03/23/KaSrPGdifgOJR2C.gif)


> 判断条件是不是`null`

![pt-null](https://i.loli.net/2021/03/23/hSbWAwPaoZeQmTG.gif)


## 参考

- [Live templates-IDEA](https://www.jetbrains.com/help/idea/using-live-templates.html)
