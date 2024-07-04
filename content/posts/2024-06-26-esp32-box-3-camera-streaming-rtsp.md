---
title: 利用esp32-box-3，实现摄像头实时流(rtsp版本)
date: 2024-07-03
draft: false
tags: ["firmware"]
---

# 如何利用esp32-box-3，实现摄像头实时流(rtsp版本)

尝试使用典型的直播协议实现摄像头实时流，如rtmp，WebRTC，rtsp等
rtmp方案里，video codec基本要求是H.264，配置s3的编码器为H.264后，拉流过来显示异常。

又开始尝试rtsp方案，服务器尝试过zlmediakit，但是它对s3的rtsp session处理有点问题，导致rtsp握手就失败了，没办法，只能使用 easydarwin。

使用easydarwin也有一个坑，用 vlc 拉流，非常卡顿，搜索一圈后，用了IINA，问题解决，ios 端可以使用`Outplayer`软件
不过 easydarwin 太久没有维护了，谨慎使用。


## firmware

固件参考的是adf的例程，idf使用 5.2 版本，adf 用的是 master 版本。

## easydarwin

docker运行，注意云平台需要打开 udp 端口。可以先用ffmpeg推流拉流测试下部署是否正常。

__推流__
```shell
ffmpeg -re -stream_loop -1 -i ./sample.mp4 -vcodec h264 -acodec aac -f rtsp -rtsp_transport tcp rtsp://ip:554/0
```

__拉流__
```shell
ffplay -i rtsp://ip:554/1
```

## reference

- [adf例程](https://github.com/espressif/esp-adf/tree/master/examples/protocols/esp-rtsp)
- [服务端代码](https://gist.github.com/robbietree8/b3438900dbf3faca7f68c6f800b90caa)
