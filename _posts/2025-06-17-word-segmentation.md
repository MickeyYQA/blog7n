---
layout: post
title: "各大古文分词库模型效果评测"
date: 2025-06-17
tags: [开发]
comments: true
---

## 分词模型

使用《庄子·逍遥游》中的《北冥有鱼》无句读版实验分词效果。

- [`thu.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/thu.py)：清华大学的 THULAC 库，效果不是很好。「北冥有鱼其名为鲲鲲之大不知其几千里也」被分割成「北冥 / 有 / 鱼 / 其 / 名 / 为 / __鲲鲲__ / 之 / 大 / 不 / 知 / 其 / 几千 / 里 / 也」。莫名好笑的分词结果。

- [`pku.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/pku.py)：北京大学的 PKUSEG 库，不知道为什么运行不了。这个库好像有什么问题，在 `/opt/homebrew/lib/python3.13/site-packages/pkuseg/__init__.py` 处报错，说不存在一个 txt 文件。

- [`jiayan1.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/jiayan1.py)：甲言模型，用起来是最好用的一个。不过还是有一些词汇不能够很好的识别出来。

## 标注词性

使用正确分词过的《北冥有鱼》实验词性标注效果。

- [`posttagger.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/posttagger.py)：效果不错，词性标注很准确。

## 分析词库

使用《千金方》中的 txt 文件实验列词库效果。

- [`lexicon.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/千金方/lexicon.py)：效果不错，最重要的是词库结果很有趣。

## 注意事项

所有 `import` 了 `kenlm` 库的 Python 代码，需要安装 Python **3.10** 才能正常运行。KenLM **不支持 Python 3.11 及以上版本**。

具体分析见：[you7n's blog -【已解决】在 macOS 上安装 KenLM](https://blog.you7n.com/2025/02/12/kenlm-installation/)
