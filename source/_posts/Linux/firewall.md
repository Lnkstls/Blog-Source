---
title: Firewall备忘
categories: Linux
tags: 
    - Linux
---

# Firewall备忘

添加永久NAT转发：
```shell
firewall-cmd --permanent --add-forward-port=port=46801:proto=tcp:toport=46801:toaddr=10.0.0.4
firewall-cmd --reload
firewall-cmd --list-all
```