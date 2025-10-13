---
layout: post
title: 【已解决】macOS VPN在校园网环境下连接问题
date: 2025-10-13
tags: [开发]
author: you7n
comments: true
---

## 问题描述

这两天主播在学校连校园网，遇到了一个非常之诡异的现象。不开VPN能正常上外网，开了VPN却什么网都上不了了，连VPN的服务器都ping不通。用检测IP地址的网站一看，发现自己的IP在塞舌尔？？问题是我买的VPN甚至没有塞舌尔的节点。。而且其他同学之前也遇到过一样的问题，来问主包主包也没能解决，这次在自己的电脑上比较好实验，于是探索了一下。

(Seychelles img)

主包用MacBook Air M2, macOS 15.6.1, VPN软件是Clash Verge 2.3.2。（不得不吐槽一下Verge这个垃圾软件，经常自己在后台未响应，有时候还打不开UI（ '▿ ' 💧）。要不是我的VPN配置只能在Verge导入我就用Clash X了。）

最开始我就以为是Verge软件的问题，去找学校的 IT guy 解决这个问题。

- 退出 Clash Verge，但在「活动监视器」里发现仍有一个 clash-verge-service 进程；
- 在网上[一搜](https://github.com/clash-verge-rev/clash-verge-rev/issues/1613#issuecomment-2307628654)，原来这是软件的 Service Mode 服务模式。
- 尝试卸载 Clash Verge，但这个后台进程还在运行；
- 重新安装后，在模块设置中卸载 Service Mode 模块，进程才彻底消失。

主包以为问题解决了，结果周一早上到学校一试发现还是一样的问题。直到我灵光一现，把校园网忘记然后重新重置密码重新登录 就突然恢复正常了。

这让我怀疑问题不止在我电脑上。

## 问题分析

和ChatGPT老师讨论了一下，认为问题是这样的：

现在我们认为这是 Clash Verge 的 Service Mode 与校园网的 NAC 系统冲突导致的。

### Clash Verge 的 Service Mode 做了什么

在 macOS 上，Clash Verge 的 “Service Mode” 会：
- 启动一个具 root 权限的守护进程；
- 改写系统的路由表与 DNS 配置；
- 新增一个虚拟网卡接口（通常叫 `utun0`），所有流量都被转发到它上面。

一旦这个守护进程崩溃、退出不干净，或者和 Wi-Fi 的认证机制冲突，就会造成系统路由错乱。

此时 macOS 认为“所有流量都要走 VPN”，但 VPN 已经失效 → 所有请求 timeout。

### 校园网的 NAC / DHCP 机制

学校的校园网要求每个设备登录门户网站（Portal）认证，这套系统会：

- 将用户认证与设备的 MAC 地址 或 IP 地址 绑定；
- 动态分配网关与 NAT 出口；
- 若检测到“未知接口”或“异常出口”，会临时阻断访问。

Clash Verge 启动 Service Mode 时创建的虚拟网卡（utun）在校园网看来就是一个“新接口”，

于是：

> 校园网 NAC：你是谁？没认证的设备不给上网。
> macOS：我所有流量都走这个接口。
> 结果 → 所有连接 timeout。

重新登录校园网的认证页后，NAC 重新绑定了正确的接口和 NAT 出口，网络恢复正常。

### 为什么 Windows 没问题

Windows 版本的 Clash（Clash for Windows / Clash Verge for Windows）通常不会启用系统级服务模式，它修改路由的方式也更温和，使用的是 WinAPI。所以在 Windows 上 DHCP 与 NAC 仍能正确识别设备接口，不会触发封锁。

### 塞舌尔 IP 从哪里来？

那为什么 IP 显示在塞舌尔？这其实不是“VPN 连到了塞舌尔”，而是：

- 路由表错乱后，DNS 查询被错误地转发到外部或缓存服务器；
- 校园网 NAT 出口返回了一个错误的地理位置映射；
- 于是检测网站以为我来自塞舌尔。

可以理解为：网络出口乱了，地理信息也随之乱了。