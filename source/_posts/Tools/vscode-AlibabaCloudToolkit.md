---
title: vscode-AlibabaCloudToolkit
date: 2021-04-02 22:00:26
categories:
  - vscode
tags:
  - vscode
---

> 你是否有过这种需求：在开发中需要使用不可访问的网页（你懂的），而本地有Http代理端口。PS：因为Python在使用Telegram模块的时候会TimeOut，所以设置这个插件可以解决问题。

如果你有这种需求，你可能发现设置vscode的HttpProxy没有作用，这个时候就轮到Alibaba Cloud Toolkit这个插件了。

# 说明
关于这个插件的介绍，附上官方链接：
> https://help.aliyun.com/document_detail/29968.html

这里主要是使用其中的HttpProxy功能，官方的说明：
- 隔离代码与环境，简化业务调试;
  >在开发测试的过程中，产品的功能存在多种对接情况，而且对接环境复杂，再>加上安全访问，业务调试更为复杂。特别是前后端分离项目，通常会涉及跨>域、Cookie、Header鉴权以及更复杂的跨堡垒机调用等问题。Alibaba >Cloud Toolkit的HTTP Proxy功能无需修改代码，便可将业务代码与环境隔>离，将复杂的业务调试请求简化管理，大幅度提高了测试效率。
- 一键切换不同代理配置;
  >在测试环境的代码中通常会添加通过代理进行请求转发的配置，但在产品发布>时需要删除该配置，甚至涉及多套测试环境，致使本地测试再次启用比较繁>琐。Alibaba Cloud Toolkit的HTTP Proxy功能支持保存历史的HTTP Proxy>配置，即开即用，大幅度提升了测试环境复用率，降低资源成本。
- 接入简洁，便捷使用。
  >您无需安装Nginx和Charles等代理工具，只需在Visual Studio Code中搜>索Alibaba Cloud Toolkit插件并嵌入，即可实现前后端联调。

# 使用
