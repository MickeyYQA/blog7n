---
layout: post
title: 使用PlayCover在macOS上运行原神及FAQ / FAQs on Running Genshin Impact with PlayCover
date: 2025-01-05
tags: [macOS, dev]
toc:  true
author: you7n
---
在原神不支持原生macOS游戏的情况下催生出的替代解决办法——PlayCover。
{: .message }

最近开始玩原神，但是原神不支持原生macOS啊...很苦恼。用模拟器或者容器运行PC版原神效果又不尽人意。遂上网搜索解决办法找到了这个PlayCover。简单来，。PlayCover就是一款能在macOS上运行**任意iOS破壳App**的工具。

## 安装和配置

安装PlayCover相比于之前的blog内容真的是非常straightforward了。进入[PlayCover官网](https://playcover.io)直接下载。下载好直接安装。

## 配置IPA资源库

既然要运行破壳App，那总得有个破壳IPA的来源吧。

> IPA就是iOS上App的文件格式。

进到PlayCover之后可以看到左侧有添加IPA资源库的地方：

<img width="140" alt="image" src="https://github.com/user-attachments/assets/813c5c4b-5914-4b31-aec6-59a6a254913d" />

主播个人比较推荐这个IPA库来源：[DecryptDay源](https://decrypt.day/library)。直接把```https://decrypt.day/library```输入进去就好了。[1]

然后下载原神！从库下载破壳IPA比从App Store下载App慢多了，耐心等一下。

### FAQ

启动几次游戏之后你会发现有一次游戏突然闪退，随即再启动就打不开了，提示「原神发生了意外错误」之类的，请不要犹豫直接调整应用程序类型，绝对管用。（除非你和我不是同一类问题）
<img width="900" alt="image" src="https://github.com/user-attachments/assets/3ad65666-c27f-43a7-8348-5bcaf6c48b6d" />

## 参考
[1] 阿西西的日常. (n.d.). _PlayCover如何添加ipa源，实现iOS APP自由，使MacBook畅玩原神！！！_ [Video]. [https://www.bilibili.com/video/av566179835/](https://www.bilibili.com/video/av566179835/)

[2] 升级 MacOS 14、14.1 后 playCover 内闪退游戏打不开？. (2023, November 3). Zhihu. [https://zhuanlan.zhihu.com/p/664862014](https://zhuanlan.zhihu.com/p/664862014)
