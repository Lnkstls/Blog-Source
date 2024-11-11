---
title: Dockerfile使用
date: 2022-09-18 21:31:34
categories: 
    - Docker
tags: 
    - Docker
    - Dockerfile
---

# Dockerfile使用

参考：
> https://docs.docker.com/engine/reference/builder/ 官方文档
> https://yeasy.gitbook.io/docker_practice/

## 编译命令

```shell
docker build .  # 通过当前目录Dockerfile文件
docker build -f /path/to/a/Dockerfile . # 指定目录
docker build -t shykes/myapp . # 构建时指定镜像名
docker build -t shykes/myapp:1.0.2 -t shykes/myapp:latest . # 保存多个镜像
```

## 指令

指令不区分大小写，为了更好识别一般使用大写。

### FROM

```shell
FROM ubuntu:18.04 # 指定父镜像
```

### COPY

复制文件到镜像，使用格式：
- COPY [--chown=<user>:<group>] <源路径>... <目标路径>
- COPY [--chown=<user>:<group>] ["<源路径1>",... "<目标路径>"]

支持Golang通配符规则，复制文件将保留元数据，通过``--chown``参数更改。

```shell
COPY hom* /mydir/
COPY hom?.txt /mydir/

COPY --chown=55:mygroup files* /mydir/
COPY --chown=bin files* /mydir/
COPY --chown=1 files* /mydir/
COPY --chown=10:11 files* /mydir/
```

### ADD

另一种复制文件的方式，更加推荐使用``COPY``，如果``源路径``是链接会尝试去下载它，如果需要解压或者更改权限，需要再添加``RUN``命令。

最适合使用``ADD``的场合，就是所提及的需要自动解压缩的场合。

同时也支持``--chown``参数更改权限：

```shell
ADD --chown=55:mygroup files* /mydir/
ADD --chown=bin files* /mydir/
ADD --chown=1 files* /mydir/
ADD --chown=10:11 files* /mydir/
```

### RUN

RUN命令执行命令并创建新的镜像层，通常用于安装软件包。

运行原生命令，使用格式：

- RUN \<command>
- RUN ["executable", "param1", "param2"]

```shell
RUN /bin/bash -c 'source $HOME/.bashrc; \ # 可使用反斜线多行
echo $HOME'

RUN ["/bin/bash", "-c", "echo hello"] # 支持传入更多参数
```

### CMD

CMD命令设置容器启动后默认执行的命令及其参数，但CMD设置的命令能够被docker run命令后面的命令行参数替换。

与``RUN``用法一样：

- CMD \<command>
- CMD ["executable", "param1", "param2"]

### ENTRYPOINT

ENTRYPOINT配置容器启动时的执行命令（不会被忽略，一定会被执行，即使运行 docker run时指定了其他命令）。

与``RUN``用法一样：

```shell
FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT [ "curl", "-s", "http://myip.ipip.net" ] # 多数使用这种格式
```

### ENV

设置环境变量，后续命令跟容器应用都能够获取：

- ENV \<key> \<value>
- ENV \<key1>=\<value1> \<key2>=\<value2>...

这里介绍通过自定义环境变量制作多个版本镜像：

```shell
ENV NODE_VERSION 7.2.0

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-x64.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-x64.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs
```

### ARG

与``ENV``一样设置环境变量，但只在构建时生效，并且需要在``FROM``指令后使用，多构建时需要再次设置：

```shell
# 只在 FROM 中生效
ARG DOCKER_USERNAME=library

FROM ${DOCKER_USERNAME}/alpine

# 要想在 FROM 之后使用，必须再次指定
ARG DOCKER_USERNAME=library

RUN set -x ; echo ${DOCKER_USERNAME}
```

### VOLUME

定义匿名卷，防止用户忘记挂载卷：

- VOLUME ["<路径1>", "<路径2>"...]
- VOLUME <路径>

```shell
VOLUME /data
```

### EXPOSE

声明端口，为用户更好理解使用哪些端口，并在执行``docker run -P``命令时自动随机映射端口：

- EXPOSE <端口1> [<端口2>...]

### WORKDIR

指定工作路径，Dockerfile构建时使用``分层存储``，使用RUN命令不会更改执行环境,r如果目录不存在会创建目录：

- WORKDIR <工作目录路径>

```shell
WORKDIR /app

RUN echo "hello" > world.txt
```

### USER

切换指定用户，改变环境状态，影响之后的命令：

- USER <用户名>[:<用户组>]

```shell
# 建立 redis 用户，并使用 gosu 换另一个用户执行命令
# 推荐使用gosu切换
RUN groupadd -r redis && useradd -r -g redis redis
# 下载 gosu
RUN wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.12/gosu-amd64" \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true
# 设置 CMD，并以另外的用户执行
CMD [ "exec", "gosu", "redis", "redis-server" ]
```

### HEALTHCHECK

提供健康检查，用来判断服务是否运行：

- HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令
  - --interval=<间隔>：两次健康检查的间隔，默认为 30 秒
  - --timeout=<时长>：健康检查命令运行超时时间，如果超过这个时间，本次健康检查就被视为失败，默认 30 秒
  - --retries=<次数>：当连续失败指定次数后，则将容器状态视为 unhealthy，默认 3 次
  
- HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令

与``ENTRYPOINT ``格式一样，可使用shell或exec格式，令的返回值决定了该次健康检查的成功与否：0：成功；1：失败；2：保留，不要使用这个值：

```shell
FROM nginx
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
HEALTHCHECK --interval=5s --timeout=3s \
  CMD curl -fs http://localhost/ || exit 1
```

### ONBUILD

参考：
> https://yeasy.gitbook.io/docker_practice/image/dockerfile/onbuild

构建下一级镜像时才会执行命令：

```shell
FROM node:slim
RUN mkdir /app
WORKDIR /app
ONBUILD COPY ./package.json /app
ONBUILD RUN [ "npm", "install" ]
ONBUILD COPY . /app/
CMD [ "npm", "start" ]
```

### LABEL

为镜像添加元数据：

- LABEL \<key>=\<value> \<key>=\<value> \<key>=\<value> ...

```shell
LABEL org.opencontainers.image.authors="yeasy"

LABEL org.opencontainers.image.documentation="https://yeasy.gitbooks.io"
```

### SHELL

指定``RUN``、``ENTRYPOINT``、``CMD``指令的shell，Linux中默认为["/bin/sh", "-c"]：

- SHELL ["executable", "parameters"]

```shell
SHELL ["/bin/sh", "-c"]
RUN lll ; ls

SHELL ["/bin/sh", "-cex"]
RUN lll ; ls
```

## 多阶段构建

```dockerfile
FROM golang:alpine as builder

RUN apk --no-cache add git

WORKDIR /go/src/github.com/go/helloworld/

RUN go get -d -v github.com/go-sql-driver/mysql

COPY app.go .

RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

FROM alpine:latest as prod

RUN apk --no-cache add ca-certificates

WORKDIR /root/

COPY --from=0 /go/src/github.com/go/helloworld/app .

CMD ["./app"]
```

使用``AS``为某一阶段命名，只构建某一阶段镜像时：

```shell
docker build --target builder -t username/imagename:tag .
```

构建时使用``--from``从其它镜像复制文件：

```dockerfile
COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf
```