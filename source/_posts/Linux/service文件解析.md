---
title: service文件解析
categories: Linux
tags: 
    - Linux
    - LinuxService
---

```shell
[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.service
Wants=sshd-keygen.service

[Service]
EnvironmentFile=/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
Type=simple
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```



## Unit

| Unit| 启动顺序与依赖关系 |
| ------------- | ---------------------------------- |
| Description   | 当前服务简单描述                   |
| Documentation | 服务描述文档位置                   |
| After         | 设置在哪些服务后启动               |
| Before        | 设置在哪些服务前启动               |
| Wants         | 设置服务弱依赖（服务停止继续运行） |
| Requires      | 设置服务强依赖（服务停止不再运行） |



## Service

| Service         | 描述如何启动服务                                             |
| --------------- | ------------------------------------------------------------ |
| EnvironmentFile | 启动环境文件（该文件内部的key=value键值对，可以用$key的形式，在当前配置文件中获取） |
| ExecStart       | 启动服务执行命令                                             |
| ExecReload      | 重启服务执行命令                                             |
| ExecStop        | 停止服务执行命令                                             |
| ExecStartPre    | 启动服务之前执行命令                                         |
| ExecStartPost   | 启动服务之后执行命令                                         |
| ExecStopPost    | 停止服务之后执行命令                                         |
| Type            | 启动类型（如下）                                             |
| RemainAfterExit | yes，服务退出后任然运行                                      |
| KillMode        | 定义如何停止服务                                             |
| Restart         | 定义服务异常退出后如何重启                                   |
| RestartSec      | 42s，定义重启等待时间                                        |



## Install

| Install  | 定义如何安装这个配置文件，即怎样做到开机启动 |
| -------- | -------------------------------------------- |
| WantedBy | 示该服务所在的 Target                        |
> Target的含义是服务组，表示一组服务。WantedBy=multi-user.target指的是，sshd 所在的 Target 是multi-user.target。
>
> 这个设置非常重要，因为执行systemctl enable sshd.service命令时，sshd.service的一个符号链接，就会放在/etc/systemd/system目录下面的multi-user.target.wants子目录之中。
>
> Systemd 有默认的启动 Target。
>
> $ systemctl get-default
>
> multi-user.target
>
> 上面的结果表示，默认的启动 Target 是multi-user.target。在这个组里的所有服务，都将开机启动。这就是为什么systemctl enable命令能设置开机启动的原因。
>
> 使用 Target 的时候，systemctl list-dependencies命令和systemctl isolate命令也很有用。
>
> \# 查看 multi-user.target 包含的所有服务
>
> $ systemctl list-dependencies multi-user.target
>
> \# 切换到另一个 target
>
> \# shutdown.target 就是关机状态
>
> $ sudo systemctl isolate shutdown.target
>
> 一般来说，常用的 Target 有两个：一个是multi-user.target，表示多用户命令行状态；另一个是graphical.target，表示图形用户状态，它依赖于multi-user.target。官方文档有一张非常清晰的 Target 依赖关系图。



## Target文件

```shell
$ systemctl cat multi-user.target

[Unit]
Description=Multi-User System
Documentation=man:systemd.special(7)
Requires=basic.target
Conflicts=rescue.service rescue.target
After=basic.target rescue.service rescue.target
AllowIsolate=yes
```

> 注意，Target 配置文件里面没有启动命令。
>
> 上面输出结果中，主要字段含义如下。
>
> Requires字段：要求basic.target一起运行。
>
> Conflicts字段：冲突字段。如果rescue.service或rescue.target正在运行，multi-user.target就不能运行，反之亦然。
>
> After：表示multi-user.target在basic.target 、 rescue.service、 rescue.target之后启动，如果它们有启动的话。
>
> AllowIsolate：允许使用systemctl isolate命令切换到multi-user.target。



### 启动参数

|               |                      |
| ------------- | -------------------- |
| ExecStart     | 启动服务执行命令     |
| ExecReload    | 重启服务执行命令     |
| ExecStop      | 停止服务执行命令     |
| ExecStartPre  | 启动服务之前执行命令 |
| ExecStartPost | 启动服务之后执行命令 |
| ExecStopPost  | 停止服务之后执行命令 |

> 所有的启动设置之前，都可以加上一个连词号（-），表示"抑制错误"，即发生错误的时候，不影响其他命令的执行。比如，EnvironmentFile=-/etc/sysconfig/sshd（注意等号后面的那个连词号），就表示即使/etc/sysconfig/sshd文件不存在，也不会抛出错。



### Type

| simple（默认值） | ExecStart字段启动的进程为主进程                              |
| ---------------- | ------------------------------------------------------------ |
| forking          | ExecStart字段将以fork()方式启动，此时父进程将会退出，子进程将成为主进程 |
| oneshot          | 类似于simple，但只执行一次，Systemd 会等它执行完，才启动其他服务 |
| dbus             | 类似于simple，但会等待 D-Bus 信号后启动                      |
| notify           | 类似于simple，启动结束后会发出通知信号，然后 Systemd 再启动其他服务 |
| idle             | 类似于simple，但是要等到其他任务都执行完，才会启动该服务。一种使用场合是为让该服务的输出，不与其他服务的输出相混合 |



### KillMode

| control-group（默认值） | 当前控制组里面的所有子进程，都会被杀掉             |
| ----------------------- | -------------------------------------------------- |
| process                 | 只杀主进程                                         |
| mixed                   | 主进程将收到 SIGTERM 信号，子进程收到 SIGKILL 信号 |
| none                    | 没有进程会被杀掉，只是执行服务的 stop 命令         |



### Restart

| no（默认值） | 退出后不会重启                                               |
| ------------ | ------------------------------------------------------------ |
| on-success   | 只有正常退出时（退出状态码为0），才会重启                    |
| on-failure   | 非正常退出时（退出状态码非0），包括被信号终止和超时，才会重启 |
| on-abnormal  | 只有被信号终止和超时，才会重启                               |
| on-abort     | 只有在收到没有捕捉到的信号终止时，才会重启                   |
| on-watchdog  | 超时退出，才会重启                                           |
| always       | 不管是什么退出原因，总是重启                                 |

