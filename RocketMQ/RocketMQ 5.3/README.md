# RocketMQ 5.3 环境启动

`rocketmq-dashboard:2.0.1` 镜像是自己打包的

源码地址：<https://github.com/apache/rocketmq-dashboard>

打包的时候会报错，需要新加个插件（父项目有这个插件，需要覆盖版本）

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-remote-resources-plugin</artifactId>
  <version>3.3.0</version>
</plugin>
```

```dockerfile
FROM openjdk:8-jre
# 设置工作目录与时区
WORKDIR /app
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone

# 复制 RocketMQ-Dashboard JAR 文件到镜像
COPY rocketmq-dashboard-2.0.1-SNAPSHOT.jar /app/rocketmq-dashboard.jar

# 暴露默认端口（根据实际配置调整）
EXPOSE 8080

# 启动命令（动态指定 Nameserver 地址）
CMD ["java", "-jar", "rocketmq-dashboard.jar", "--rocketmq.config.proxyAddr=${PROXY_ADDR}"]
```

打包：

```shell
docker build -t rocketmq-dashboard:2.0.1 -f Dockerfile .
```
