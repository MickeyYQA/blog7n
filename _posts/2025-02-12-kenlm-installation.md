---
layout: post
title: 【已解决】在macOS上安装KenLM
date: 2025-02-12
tags: [macOS, dev]
author: you7n
---
KenLM 是一个高效的 n-gram 语言模型库，但在 Mac 上安装时遇到了各种问题。弄了一晚上加一上午终于安装好了，本文记录了成功安装 KenLM 的方法。
{: .message }

如果你使用 Python 3.10 以上版本，你可能适用这个解决办法。其他解决办法参见：

- 修改```setup.py```：[Zhihu/98137373](https://zhuanlan.zhihu.com/p/98137373)
- 修改Xcode头文件：[Zhihu/631044108](https://zhuanlan.zhihu.com/p/631044108)

尝试使用```pip```或```pip3```安装```kenlm```的时候很可能会失败。大概原因从GitHub的issues中看，似乎是pip那边的kenlm库有某种问题(?)

直接```pip3 install kenlm```或者```pip3 install pypi-kenlm```会这样报错：

<img width="815" alt="image" src="https://github.com/user-attachments/assets/90356ac3-40bc-49e8-af17-b25eeac559d5" />

主要表现为

```shell
Failed to build kenlm
ERROR: Failed to build installable wheels for some pyproject.toml based projects (kenlm)
```


所以尝试使用GitHub版手动编译。

```shell
git clone https://github.com/kpu/kenlm
```

cd进入kenlm文件夹
```shell
cd /Users/■■■/kenlm
```

KenLM 无法在最新的 Python 3.13 版本下正常编译，因此使用 Python 3.10。

可以通过```brew```安装：

```shell
brew install python@3.10
```

重新创建 python3 软链接啊、设置全局 Python 版本都太麻烦了。直接通过```pip3.10```安装本地库。

因为刚才已经cd进了kenlm文件夹，直接安装本文件夹：

```shell
pip3.10 install .
```
此时应该安装成功，可以测试一下。```kenlm/python/example.py```是一个测试文件，用python3.10运行：

```shell
python3.10 /Users/■■■/kenlm/python/example.py 
```

如图运行结果则成功：

<img width="756" alt="image" src="https://github.com/user-attachments/assets/c36b964e-b17a-4231-8159-95b9a10f68a2" />

以上。
