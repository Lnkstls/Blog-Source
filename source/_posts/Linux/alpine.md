---
title: alpine
date: 2023-09-22 17:25:59
categories:
    - Linux
tags:
    - alpine
---

# alpine

alpine是一个开源、轻量的Linux系统

## 包管理系统

更换阿里镜像源
```shell
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
```

安装
> apk add *