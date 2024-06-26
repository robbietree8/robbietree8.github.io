---
title: Firmware developer guide using MicroPython
date: 2022-05-07
draft: false
tags: ["firmware"]
---

# 固件开发指北

笔者本身主业是后端开发，机缘巧合之下，需要开发一款固件，按照极客时间里的课程摸索着也做出来了，现将过程中的一些经验分享如下。


## 开发语言

`C/C++`是开发固件的首选语言，
由于`python`的流行以及上手简单，也有用`MicroPython`的
如果让我在`MicroPython`之外选择一门语言，我会考虑一下`rust`
对语言的选择需要考虑，1.生态（对流行的芯片有没有现成的库可用用），2.社区（活跃程度，遇到问题能不能解决）


## 开发板

esp32作为近来比较流行的一款芯片，价格比较低，功能也比较强大，集成了wifi和蓝牙，是入门的首选。


## 开发工具

可以在vscode里开发固件，安装`Pymakr`插件


## 开始动手

### REPL

mac下可以使用`screen`连上开发板就可以进入repl模式

```sh
❯ screen /dev/cu.usbserial-1420 115200,cs8
```

```python
print('hello')
```

```python
from machine import Pin
pin = Pin(2)
pin.on()
pin.off()
```

### 上传/运行文件

```python
from machine import Pin
import time

pin = Pin(2)
while True:
  pin.on()
  time.sleep(1)
  pin.off()
  time.sleep(1)
```

将上述的代码保存为`blink.py`，通过Pymakr插件运行此文件


## 进阶

### lib开发

对于真实的项目来说，你可能需要针对一款芯片写一些驱动代码
你可以将这些驱动代码开源


### 固件编译

你可能需要将你自己写的代码与`MicroPython`编译到一个文件里，方便量产烧录


### OTA升级

硬件是固化的，但是软件是可以升级的
那么你怎么升级硬件里的软件呢，也就是固件?

`OTA`指的是可以通过空中升级的方式来升级硬件里的固件，比如通过wifi或者蓝牙


### 日常调试

调整参数的时候可以多多利用wifi或者蓝牙，这样就不用拷文件或烧录了


## 其他考虑

## 器件

即使是开发固件，工作中也会遇到需要使用一些器件的情况，最通常的就是电压表了。
此外直流电源，示波器，逻辑分析仪也会用到。


## 元器件

对于真实的项目来说，你可能需要购买元器件，一般直接在淘宝上搜就可以了，哪个便宜买哪个，如果要买的东西比较多，比较杂的话，也可以考虑立创商城，它会打包寄给你。



## 参考

- [极客时间的教程](https://time.geekbang.org/column/intro/100063601)
- [固件编译](https://github.com/unseel/docker-micropython-tools-esp32)
