---
title: 开始使用
categories: [Linux, shell]
tags: Hexo
---

官方文档：https://hexo.io/zh-cn/docs/

# 安装NodeJs与Npm

```shell
apt -y install nodejs npm
```



# 安装Hexo-cli

```shell
npm install hexo-cli -g
```



# 初始化

```shell
hexo init blog
npm install
```



# 开始体验

```shell
hexo server
```

# 常用命令

## new

创建一个新的文章：

- -p, --path 自定义新文章的路径

```shell
hexo new [layout] <title>

hexo new page -p path/.md
```

默认使用配置文件中的``default_layout``参数，包含空格使用引号括起来。


## 标签

使用资源文件：

```yaml
post_asset_folder: true # 开启资源文件夹

# 标签
{% asset_path fileName %}
{% asset_img fileName [title] %}
{% asset_link fileName [title] %}
```

iframe：

```yaml
{% iframe url [width] [height] %}
```

文章摘要和截断：

```yaml
<!-- more -->
```