# 服务器文件操作笔记

__数据库备份__

```shell
mysqldump -uroot -p -hlocalhost -P3306 db > /var/lib/mysql/db.sql
```

__数据库恢复__

```shell
mysql -uroot -p -hlocalhost -P3306 db < db.sql
```

__文件打包__

```shell
tar -zcf /dst/foo.tar.gz foo
```

这里有个技巧，尽量按少量的文件夹打包，一来可以缩小打包后的文件大小，二来单次打包速度会较快。
`tar`操作会比较耗`CPU`资源，所以在操作的时候，可以把其他耗`CPU`的服务先暂停掉。

__文件传输__

```shell
scp user@host:/dst/foo.tar.gz .
```

__其他__

服务器上操作可以用下`tmux`神器，帮助你管理`session`，不至于打包到一半的时候连接断了。
服务器操作，慎用`rm`。
