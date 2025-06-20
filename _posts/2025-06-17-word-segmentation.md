---
layout: post
title: "各大古文分词库模型效果评测"
date: 2025-06-17
tags: [开发]
comments: true
---

在尝试使用甲言模型给手里的《千金方》古文做词频统计的时候，顺手测试了一下比较有名的几个古文分词/词性/词库模型的效果。

## 模型测试

### 分词模型

分词模型的作用是把一段无句读的古文文本分成一个一个词，这里我们使用《庄子·逍遥游》中的《北冥有鱼》无句读版实验分词效果。

> 北冥有鱼其名为鲲鲲之大不知其几千里也化而为鸟其名为鹏鹏之背不知其几千里也怒而飞其翼若垂天之云是鸟也海运则将徙于南冥南冥者天池也齐谐者志怪者也谐之言曰鹏之徙于南冥也水击三千里抟扶摇而上者九万里去以六月息者也

- [`thu.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/thu.py)：清华大学的 THULAC 库，效果不是很好。「北冥有鱼其名为鲲鲲之大不知其几千里也」被分割成「北冥 / 有 / 鱼 / 其 / 名 / 为 / __鲲鲲__ / 之 / 大 / 不 / 知 / 其 / 几千 / 里 / 也」。莫名好笑的分词结果。

- [`pku.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/pku.py)：北京大学的 PKUSEG 库，不知道为什么运行不了。这个库好像有什么问题，在 `/opt/homebrew/lib/python3.13/site-packages/pkuseg/__init__.py` 处报错，说不存在一个 txt 文件。

- [`jiayan1.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/jiayan1.py)：甲言模型，用起来是最好用的一个。不过还是有一些词汇不能够很好的识别出来。

### 标注词性

类似地，使用正确分词过的《北冥有鱼》实验词性标注效果。

- [`posttagger.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/posttagger.py)：效果不错，词性标注很准确。

### 分析词库

使用《千金方》中的 txt 文件实验列词库效果。

- [`lexicon.py`](https://github.com/MickeyYQA/classical-chinese-word-segmentation/blob/main/千金方/lexicon.py)：效果不错，最重要的是词库结果很有趣。

## 注意事项

所有 `import` 了 `kenlm` 库的 Python 代码，需要安装 Python __3.10__ 才能正常运行。KenLM __不支持 Python 3.11 及以上版本__。

具体分析见：[you7n's blog -【已解决】在 macOS 上安装 KenLM](https://blog.you7n.com/2025/02/12/kenlm-installation/)
