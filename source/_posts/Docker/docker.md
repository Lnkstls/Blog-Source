---
title: Docker的基本操作使用
categories: [Linux, Docker]
tags: [Docker]
---



# 关于Dokcer



# 镜像

docker镜像都是只读的，当docker启动时将新建一个可写层到顶部。

这一层通常称为容器层，之下为镜像层。

> https://hub.docker.com/  Docker官方仓库

## Images：查看镜像

> docker images [OPTIONS] [REPOSITORY[:TAG]]

| OPTIONS    |                                                              |
| ---------- | ------------------------------------------------------------ |
| -a         | 列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）。 |
| -f         | 显示满足条件的镜像。                                         |
| -q         | 只显示镜像ID。                                               |
| --digests  | 显示镜像的摘要信息.                                          |
| --format   | 指定返回值的模板文件。                                       |
| --no-trunc | 显示完整的镜像信息。                                         |

## Rmi：删除镜像

> docker rmi [OPTIONS] IMAGE [IMAGE...]

| OPTIONS    |                                    |
| ---------- | ---------------------------------- |
| -f         | 强制删除。                         |
| --no-prune | 不移除该镜像的过程镜像，默认移除。 |

## Tags：标记镜像

> docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]

## Build：创建镜像

> docker build [OPTIONS] PATH | URL | -

## History：查看创建历史

> docker history [OPTIONS] IMAGE

| OPTIONS    |                                              |
| ---------- | -------------------------------------------- |
| -H         | 以可读的格式打印镜像大小和日期，默认为true。 |
| -q         | 仅列出提交记录ID。                           |
| --no-trunc | 显示完整的提交记录。                         |

## Save：将指定镜像保存为tar

> docker save [OPTIONS] IMAGE [IMAGE...]

| OPTIONS |                |
| ------- | -------------- |
| -o      | 输出到的文件。 |

## Load：导入tar文件镜像

| OPTIONS     |                              |
| ----------- | ---------------------------- |
| -i, --input | 指定导入的文件，代替 STDIN。 |
| -q, --quiet | 精简输出信息。               |

## Import：从归档文件中创建镜像

> docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]

| OPTIONS |                           |
| ------- | ------------------------- |
| -c      | 应用docker 指令创建镜像。 |
| -m      | 提交时的说明文字。        |



# 镜像仓库



## Login/Logout：登录/退出

> docker login [OPTIONS] [SERVER]

> docker logout [OPTIONS] [SERVER]

| OPTIONS |                |
| ------- | -------------- |
| -u      | 登陆的用户名。 |
| -p      | 登陆的密码。   |

## Pull：拉取镜像

> docker pull [OPTIONS] NAME[:TAG|@DIGEST]

| OPTIONS                 |                           |
| ----------------------- | ------------------------- |
| -a                      | 拉取所有 tagged 镜像。    |
| --disable-content-trust | 忽略镜像的校验,默认开启。 |

## Push：推送镜像

推送需要先登录。

> docker push [OPTIONS] NAME[:TAG]

| OPTIONS                 |                           |
| ----------------------- | ------------------------- |
| --disable-content-trust | 忽略镜像的校验,默认开启。 |

## Search：查找镜像

> docker search [OPTIONS] TERM

| OPTIONS     |                                    |
| ----------- | ---------------------------------- |
| --automated | 只列出 automated build类型的镜像。 |
| --no-trunc  | 显示完整的镜像描述。               |
| -s          | 列出收藏数不小于指定值的镜像。     |



# 容器

## Ps：查看容器

> docker ps [OPTIONS]

| OPTIONS    |                                |
| ---------- | ------------------------------ |
| -a         | 显示所有的容器，包括未运行的。 |
| -f         | 根据条件过滤显示的内容。       |
| -q         | 静默模式，只显示容器编号。     |
| -s         | 显示总的文件大小。             |
| -l         | 显示最近创建的容器。           |
| -n         | 列出最近创建的n个容器。        |
| --no-trunc | 不截断输出。                   |
| --format   | 指定返回值的模板文件。         |

- CONTAINER ID：容器ID.
- IMAGE：使用的容器。
- COMMAND：启动容器运行的命令。
- CREATED：容器的创建时间。
- STATUS：容器状态。
  - created（已创建）
  - restarting（重启中）
  - running（运行中）
  - removing（迁移中）
  - paused（暂停）
  - exited（停止）
  - dead（死亡）
- PORTS：容器的端口信息和使用的连接类型（tcp\udp）。
- NAMES：自动分配的容器名称。

## Inspect：查看容器/镜像元数据

> docker inspect [OPTIONS] NAME|ID [NAME|ID...]

| OPTIONS |                        |
| ------- | ---------------------- |
| -f      | 指定返回值的模板文件。 |
| -s      | 显示总的文件大小。     |
| --type  | 为指定类型返回JSON。   |

## Top：查看容器进程

> docker top [OPTIONS] CONTAINER [ps OPTIONS]

## Attach：连接运行中的容器

> docker attach [OPTIONS] CONTAINER

要attach上去的容器必须正在运行，可以同时连接上同一个container来共享屏幕（与screen命令的attach类似）。

官方文档中说attach后可以通过CTRL-C来detach，但实际上经过我的测试，如果container当前在运行bash，CTRL-C自然是当前行的输入，没有退出；如果container当前正在前台运行进程，如输出nginx的access.log日志，CTRL-C不仅会导致退出容器，而且还stop了。这不是我们想要的，detach的意思按理应该是脱离容器终端，但容器依然运行。好在attach是可以带上--sig-proxy=false来确保CTRL-D或CTRL-C不会关闭容器。（引用菜鸟）

## Events：查看事件

> docker events [OPTIONS]

| OPTIONS |                                |
| ------- | ------------------------------ |
| -f      | 根据条件过滤事件。             |
| --since | 从指定的时间戳后显示所有事件。 |
| --until | 流水时间显示到指定的时间为止。 |

## Logs：查看容器日志

> docker logs [OPTIONS] CONTAINER

| OPTIONS |                              |
| ------- | ---------------------------- |
| -f      | 跟踪日志输出。               |
| -t      | 显示时间戳。                 |
| --tail  | 仅列出最新N条容器日志。      |
| --since | 显示某个开始时间的所有日志。 |

## Wait：阻塞容器运行

> docker wait [OPTIONS] CONTAINER [CONTAINER...]

## Export：导出tar文件到STDOUT

> docker export [OPTIONS] CONTAINER

| OPTIONS |                      |
| ------- | -------------------- |
| -o      | 将输入内容写到文件。 |

## Port：查看容器端口

> docker port [OPTIONS] CONTAINER [PRIVATE_PORT[/PROTO]]