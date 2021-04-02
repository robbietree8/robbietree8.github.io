# Docker里如何使用jmap

## 缘由

排查cpu 100%问题的时候，发现一个有趣的现象，使用openjdk:8-jdk-alpine作为基础镜像的应用，无法使用jmap, jstack等jdk的工具，但是使用openjdk:8-jdk作为基础镜像，却可以使用，即使应用的PID为1。



## 测试

> 使用alpine镜像

```
FROM openjdk:8-jdk-alpine

COPY ./target /opt/target
WORKDIR /opt/target
RUN find -type f -name "*.jar" | xargs -I{} mv {} app.jar
ENTRYPOINT exec java -Djava.security.egd=file:/dev/./urandom -jar app.jar
```

__构建镜像__

```
docker build -t docker-jmap .
docker run --name docker-jmap-test --rm -it docker-jmap:latest
docker exec -it docker-jmap-test sh
```

![](https://i.loli.net/2018/11/06/5be17b294bf56.jpg)

`使用alpine镜像不加--init参数的时候，无法使用jdk工具`

__加`--init`__

```
docker run --name docker-jmap-test --rm --init -it docker-jmap:latest
```

docker镜像里java应用的进程号不为1，可以使用`jstack`

![](https://i.loli.net/2018/11/06/5be17b2b74f38.jpg)

`使用alpine镜像加上--init参数的时候，可以使用jdk工具`


> 使用完整镜像

```
FROM openjdk:8-jdk

COPY ./target /opt/target
WORKDIR /opt/target
RUN find -type f -name "*.jar" | xargs -I{} mv {} app.jar
ENTRYPOINT exec java -Djava.security.egd=file:/dev/./urandom -jar app.jar
```

__构建镜像__

```
docker build -t docker-jmap-full .
docker run --name docker-jmap-test --rm -it docker-jmap-full:latest
docker exec -it docker-jmap-test sh
```

![](https://i.loli.net/2018/11/06/5be17b2b39312.jpg)

`使用完整镜像即使不加--init参数的时候，也可以使用jdk工具`


> 使用`tini`

如果是在`k8s`里部署应用的话，`--init`参数无法使用，可以尝试用`tini`来解决

```
FROM openjdk:8-jdk-alpine

ENV JAVA_OPTS="-Xms2g -Xmx4g"
COPY ./target /opt/target
WORKDIR /opt/target
RUN find -type f -name "*.jar" | xargs -I{} mv {} app.jar

# Add Tini
RUN apk add --no-cache tini
ENTRYPOINT ["/sbin/tini", "-s", "--"]
CMD java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar app.jar
```

__构建镜像__

```
docker build -t docker-jmap-full .
docker run --name docker-jmap-test --rm -it docker-jmap-full:latest
docker exec -it docker-jmap-test sh
```

![](https://i.loli.net/2018/11/08/5be3fd03695a0.jpg)


## 参考资料

[Tini - 一个小而有效的容器初始化命令](http://yunke.science/2018/04/09/Tini-command/)
