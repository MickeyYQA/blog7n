---
layout: post
title: 如何在64位macOS上运行32位Portal / Guide to Installing 32-bit Portal on 64-bit macOS
date: 2024-10-19
tags: [macOS, game, portal, dev]
toc:  true
author: you7n
math: true
---
一款适用于macOS15及以下的、在macOS上运行32位游戏的教程。
{: .message }

如果你在寻找下面这条提示的解决办法，那你来对地方了。
<img width="952" alt="image" src="https://github.com/user-attachments/assets/f74270b7-beab-4e95-9d3d-9bacca090ada">

GitHub大神们开发出了能在macOS上运行32位游戏的方法。由于本蒟蒻看不懂 本篇不讨论原理。像研究原理的佬们请移步[>>>GitHub库源地址<<<](https://github.com/nillerusr/source-engine)。

## 安装前置包

如果你之前喜欢捣鼓电脑或者已经安装过Homebrew和Xcode Command Line Tools，可以跳过1-4步。

### 1. 安装Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2a. 如果你在用的是Intel芯片的mac:

如果你在用的是Intel芯片的mac，用这条。注意一定要把「yourusername」替换成你自己的用户名！

```bash
(echo; echo 'eval "$(/usr/local/bin/brew shellenv)"') >> /Users/yourusername/.zprofile

eval "$(/usr/local/bin/brew shellenv)"
```

    
### 2b. 如果是A系列芯片的mac：

```bash
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/yourusername/.zprofile

eval "$(/opt/homebrew/bin/brew shellenv)"
```
    
### 3. 安装Xcode Command Line Tools。

时间可能会比较长
   
```bash
xcode-select --install
```

### 4. 使用homebrew安装必要的库。

```bash
brew help

brew doctor

brew cleanup

brew install sdl2 freetype2 fontconfig pkg-config opus jpeg jpeg-turbo libpng libedit python3
```

## 下载、编译代码

### 5. Git Clone源代码

注意「--recursive」是不能删掉的。

```bash
git clone --recursive https://github.com/nillerusr/source-engine
```

随后cd进clone的文件夹。

```bash
cd source-engine
```

### 6. Configure portal build

```bash
python3 waf configure -T release --prefix='' --build-games=portal
```

如果这一步build失败了，检查一下第五步git clone下来的对不对，有没有加「--recursive」。

### 7. Build

```bash
python3 waf build
```

可能需要五分钟左右 耐心等待。

### 8. 安装软件到随便一个文件夹。

把「/Path/to/your/folder」替换成你要安装游戏的位置。比如/Users/username/Documents/gaming

```bash
python3 waf install --destdir='/Path/to/your/folder'
```

## 下载-转移游戏文件

### 9. 打开Steam，安装portal游戏本体。

### 10. 转移游戏文件

接下来开始有点tricky了——一定要认真检查

首先打开两个Finder窗口，一个是你刚才安装游戏的「/Path/to/your/folder」位置，一个是Steam游戏安装的位置，路径为'/Users/username/Library/Application Support/Steam/steamapps/common/Portal'。
在这里，我先称其为「自定义文件夹」——也就是前者——和「Steam文件夹」——后者。

默认情况下，/Users/username/Library/这个文件夹是隐藏的，但是你可以通过使用「前往文件夹」打开这个文件夹。
<img width="261" alt="image" src="https://github.com/user-attachments/assets/bbdd73bd-75b7-4342-b056-32387f1df4e7">

接下来，把**Steam文件夹里的/portal**中除了bin的所有文件夹（意思是/common/Portal/portal里面的所有文件夹）都复制到**自定义文件夹的/portal**。注意大小写！如果不明白可以看图：
<img width="922" alt="image" src="https://github.com/user-attachments/assets/ce236bc4-fbd1-49bb-897f-641aa7a9c4f8">

然后把Steam文件夹（上一部的上层文件夹, /common/Portal）中的platform文件夹和hl2文件夹**拖到**自定义文件夹里。

## 制作启动器

### 11. 使用Automator制作启动器

打开Automator，新建一个应用程序。

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/b7a9360d-5dfa-42fd-936e-a547a1b336e6">

把运行Shell脚本模块中的代码改为

```shell
cd /Path/to/your/folder

./hl2_launcher -game portal
```

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/255e1d25-7c6d-4f4c-a244-d39a0995e020">

保存这个Automator随便到哪个文件夹，只要你平时方便从这里点开游戏就可以。

到这一步你的Portal就已经可以顺利打开了（但愿如此）。只需要点击你保存到Automator，就能顺利启动游戏了！

<img width="1470" alt="image" src="https://github.com/user-attachments/assets/9919a558-311b-4672-94d6-71c6a4bc0cf8">

## 参考

[_Guide to Installing Portal Using Source Engine on macOS_ (2023-08-04). O luwadare Aganga-Williams. https://jxhug.notion.site/Guide-to-Installing-Portal-Using-Source-Engine-on-macOS-660803f9ced149cfa1647d38fd5a7092](https://jxhug.notion.site/Guide-to-Installing-Portal-Using-Source-Engine-on-macOS-660803f9ced149cfa1647d38fd5a7092)
