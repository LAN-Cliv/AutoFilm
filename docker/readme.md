# Akimio521/AutoFilm

一个为Emby、Jellyfin服务器提供直链播放的小项目

## 部署指南

首先确保你的系统上已经安装了 Docker。

- [Docker 安装文档](https://docs.docker.com/engine/install/)

### 拉取镜像

```bash
docker pull XXXXX
```

### 或者构建容器

将项目中的 `dockerfile`、`entrypoint.sh` 文件储存在本地后，执行以下指令

```bash
docker build -t autofilm .
```

### 运行容器

使用以下命令来启动一个名为 `autofilm` 的容器：

```bash
docker run --name autofilm -d -v ./config:/app/config -v ./media:/app/media -e INTERVAL=3600 -e AUTO_UPDATE=false -e CN_UPDATE=false autofilm
```

这个命令会将当前目录下的 config 和 media 目录挂载到容器内的相应位置，以便容器可以访问这些文件。

### 也可以采用 `docker compose` 的方式

```bash
version: '3.8'

services:
  autofilm:
    image: autofilm
    container_name: autofilm
    volumes:
      # 映射本地config目录
      - ./config:/app/config
      # 生成的Strm文件将会保存在media目录中
      - ./media:/app/media
    restart: unless-stopped
    environment:
      # 设定主程序循环执行时间间隔，为0时表示仅运行一次
      - INTERVAL=3600
      # 设定是否需要自动更新,默认为false,开启则填true
      - AUTO_UPDATE=false
      # 设定是否需开启代理地址进行更新,默认为false,开启则填true
      - CN_UPDATE=false
```
保存为docker-compose.yaml文件储存在本地

- 首次执行可以使用 `docker compose up` 如果执行失败可尝试  `docker-compose up` 

- 查看日志无问题后 `CTRL+C` 中止docker运行

- 再次执行 `docker compose up -d` 转为后台运行


## 查看日志

如果你需要查看应用的输出或检查错误，可以使用以下命令查看日志：

```bash
docker logs autofilm
```
## 配置说明

### 为确保 AutoFilm 正确运行，请按照以下步骤配置 config.yaml 文件：

- 1.确保 config.yaml 文件位于你的 config 目录中。

- 2.根据你的需求调整配置参数。

- 3.在更改配置文件后，你需要重启容器以应用新的配置设置。

## 联系方式

如果你在使用 AutoFilm 时遇到问题，或者有任何建议，请提交issues。

- [创建 Issue](https://github.com/Akimio521/AutoFilm/issues)
