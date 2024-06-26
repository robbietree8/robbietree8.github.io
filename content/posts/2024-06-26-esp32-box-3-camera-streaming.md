---
title: 利用esp32-box-3，实现摄像头实时流
date: 2024-06-26
draft: false
tags: ["firmware"]
---

# 如何利用esp32-box-3，实现摄像头实时流

## firmware

摄像头捕捉到一帧的数据后，拆包后通过 udp 传送到服务端


## udp receiving

接收 udp 的分包数据，按照字节头判断是否为图片，合并之后保存成 bytearray

## website

利用`multipart/x-mixed-place;boundary=xxxx`的方式，实现实时图片流


## troubleshooting

完成在本机调试后，将服务端代码部署至阿里云ecs，固件 udp 推送到服务器 ip，结果遇到了服务器没有接受到 udp 包的问题，以下是尝试的方案。

1. 首先确认安全组里打开了 udp 的端口
2. 使用 nc 开启监听
    ```shell
    nc -4 -u -l 3333
    ```
3. 使用 echo 命令推送数据
   ```shell
   echo "Hello from PC" | nc -w1 -u ${ip} 3333
   ```
4. 确认端口响应没有问题后，尝试使用 python 开启 udp 监听
    ```python
    import socket
    import sys

    # Create a TCP/IP socket
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    # Bind the socket to the port
    server_address = ('0.0.0.0', 3333)
    print >>sys.stderr, 'starting up on %s port %s' % server_address
    sock.bind(server_address)

    while True:
        print >>sys.stderr, '\nwaiting to receive message'
        data, address = sock.recvfrom(4096)
        
        print >>sys.stderr, 'received %s bytes from %s' % (len(data), address)
        print >>sys.stderr, data
        
        if data:
            sent = sock.sendto(data, address)
            print >>sys.stderr, 'sent %s bytes back to %s' % (sent, address)
    ```
5. 确认 python 接收没有问题后，就可以开始怀疑是 docker 的问题了
6. 修改 ports 转发，在端口后加上`/udp`就可以了
   ```yml
    ports:
    - "3000:3000"
    - "3333:3333/udp"
   ```

## reference

- [固件代码](https://github.com/robbietree8/esp-box-camera-streaming)
- [服务端代码](https://github.com/robbietree8/udp-streaming-server-docker)
