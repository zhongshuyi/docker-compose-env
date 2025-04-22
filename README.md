# docker-compose-env

 各种环境配置 docker compose，本地快速启动开发环境

## 背景

开发环境搭建是一个比较麻烦的事情，需要安装各种软件，配置各种环境，每次都需要重新配置，很麻烦，所以我就想把这些环境都放到 docker 中，方便快速启动，方便开发。

## 安装

使用本仓库前，需要安装 docker 和 docker compose

Windows 可以使用 Docker Desktop，

文档：<https://docs.docker.com/desktop/setup/install/windows-install/>

Linux 安装文档

文档：<https://docs.docker.com/engine/install/>

Docker Desktop 安装后会自动安装 docker compose

linux 需要自行安装 docker compose

docker-compose：[下载地址](https://github.com/docker/compose/releases)

比如下载 docker-compose-linux-x86_64

移动到 `/usr/local/bin/`

```shell
sudo mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose
```

添加执行权限

```shell
sudo chmod +x /usr/local/bin/docker-compose
```

验证安装

```shell
docker-compose --version
```

## 用法

克隆仓库

```shell
git clone https://github.com/zhongshuyi/docker-compose-env.git
```

进入你需要运行的环境目录，执行

```shell
docker compose up -d
```

如果需要停止环境，执行

```shell
docker compose down
```

## 附加内容

### win 下载 docker 镜像

使用 dget 可以下载镜像

项目地址：<https://gitee.com/extrame/dget>

下载之后传到服务器后，使用 `docker load -i xxx.tar.gz` 加载镜像

## 主要维护人员

[@钟舒艺](https://github.com/zhongshuyi)

## 参与贡献方式

详细请阅读文档 [参与贡献](https://github.com/zhongshuyi/developer-knowledge-base/blob/main/%E5%85%B6%E4%BB%96/%E5%8F%82%E4%B8%8E%E8%B4%A1%E7%8C%AE.md)
与 [协作开发流程](https://github.com/zhongshuyi/developer-knowledge-base/blob/main/%E5%B7%A5%E5%85%B7/Git/Git%20%E5%8D%8F%E4%BD%9C%E5%BC%80%E5%8F%91%E6%B5%81%E7%A8%8B%E4%B8%8E%E8%A7%84%E8%8C%83.md)
并按其中的规范与流程进行贡献

提问请到 [Issues](https://github.com/zhongshuyi/docker-compose-env/issues)

接受
PR，具体请看 [协作开发流程](https://github.com/zhongshuyi/developer-knowledge-base/blob/main/%E5%B7%A5%E5%85%B7/Git/Git%20%E5%8D%8F%E4%BD%9C%E5%BC%80%E5%8F%91%E6%B5%81%E7%A8%8B%E4%B8%8E%E8%A7%84%E8%8C%83.md)

## 许可证

[MIT](LICENSE) © 2025 [zhongshuyi](https://github.com/zhongshuyi)
