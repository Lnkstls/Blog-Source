---
layout: posts
title: vnstat的使用
date: 2020.12.26
categories: 
  - Linux
tags: 
  - Linux
---



# Vnstat解析

> vnstat [OPTION]

| OPTION          |                            |
| --------------- | -------------------------- |
| -q, --query     |                            |
| -h, hours       | 显示小时流量。             |
| -d, --days      | 显示日流量。               |
| -m, --months    | 显示月流量。               |
| -w, --weeks     | 显示周流量。               |
| -t, --top10     | 显示流量最高的10天。       |
| -s, --short     | 使用简略输出。             |
| -u, --update    | 更新数据库。               |
| -i, --iface     | 选择网卡接口（默认eth0）。 |
| -?, --help      | 查看帮助。                 |
| -v, --version   | 查看版本信息。             |
| -tr, --traffic  |                            |
| -ru, --rateunit |                            |
| -l, --live      | 显示实时流量。             |
| -r, --reset     | 重置接口统计。             |
| -D, --debug     |                            |
| --oneline       | 显示简单可解析格式。       |
| --importdb      | 导入以前导出的数据库。     |
| --json          | 以json格式显示。           |
| --xml           | 以xml格式显示。            |
| --create        | 创建数据库。               |
| --delete        | 删除数据库。               |
| --sync          | 同步计数。                 |
| --enable        | 启用接口。                 |
| --disable       | 关闭接口。                 |
| --nick          | 为接口设置昵称。           |
| --cleartop      | 清除前10名。               |
| --rebuildtotal  | 重置月份的传输数。         |
| --style         | 选择输出样式（0-4）        |
| --iflist        | 显示可用接口列表。         |
| --dbdir         | 选择数据库目录。           |
| --locale        | 设置区域设置。             |
| --config        | 选择配置文件。             |
| --savemerged    | 将数据库保存到当前目录。   |
| --showconfig    | 使用当前设置存储配置文件。 |
| --testkernel    | 检查内核是否损坏。         |
| --longhelp      | 查看所有帮助。             |
| --dumpdb        | 输出数据库。               |

> TX代表上传数据，RX是接收数据。

## --dumpdb

> vnstat --dumpdb

格式：

> d;0;1078696800;559;7433;68;557;1

其中 d = 天，0 = 数据库中的天数 （0 为今天），1077314401 日期在 Unix 时间， 559 = rx MiB， 7433 = tx MiB， 68 = rx KiB， 557 = tx KiB 和 1 告诉 vnStat 已填充此值，并且正在使用中。

> m;0;1078092000;48649;139704;527;252;1	（x12）
> t;0;1078351200;5979;47155;362;525;1	（x10）
> h;0;1078699800;118265;516545	（x24）

m = 月，t = top10 和 h = 小时，所有其他字段的顺序与天相同，除非小时没有单独的 KiB 值。在几个小时内，第四个和第五个字段在 KiB 中具有值。



## 清零

数据都在 /var/lib/vnstat/网卡 下，直接删除。



## --delete/--create

> vnstat --delete --force -i eth0
>
> vnstat --create -i eth0

删除数据库重新统计。



## 其它

### 不自动刷新

更改权限组：

> chown -R vnstat:vnstat /var/lib/vnstat