---
layout: post
title: 一款能展示Web页面的屏幕保护程序：WebViewScreenSaver（附使用例） / MacOSX Screen Saver powered by Web Pages - WebViewScreenSaver
date: 2024-11-02
tags: [macOS, dev]
toc:  true
author: you7n
---
这个好玩——能展示Web页面的macOS屏保程序。/ Mac OS X Screen Saver powered by a Web View
{: .message }

因为一直以来都是下载别人做好的Screensaver。比如之前下了一个叫WhatColourIsIt的，会调用时间（比如10:09:00），转化为hex值对应的颜色（#100900），并显示为背景。但是这个Screensaver的效果不好，因为它的背景颜色不会每秒更新，显示的一直是进入屏保的首个颜色。于是就想到，这玩意也不是很难不如自己做一个。然后今天上午发现了[>>这个<<](https://github.com/liquidx/webviewscreensaver?tab=readme-ov-file)神奇的小工具，对我来说超有用。

## 安装 WebViewScreenSaver

使用brew：
```bash
brew install --cask webviewscreensaver --no-quarantine
```
或者直接从[release](https://github.com/liquidx/webviewscreensaver/releases)里面下载（推荐）。

非常简单。

## 配置 WebViewScreenSaver

进入刚刚下载文件的地方，通常是Download文件夹，双击打开.saver文件。

<img width="594" alt="image" src="https://github.com/user-attachments/assets/5f0368d3-2700-4180-8710-967d57add6bc">

进入设置，进入该屏保选项

<img width="705" alt="image" src="https://github.com/user-attachments/assets/1331b8e2-3f5e-4441-92bd-28b0203e0cca">

这里可以输入URL，也可以设置本地html文件。但是需要注意要形如`file:///Users/admin/index.html`这样。

## 使用例-HEX Clock

这就是上面提到的自己做的屏保哈哈哈哈 很简单的html但是效果还不错。还可以配合hot corner一起使用，把鼠标拨拉到角上直接进screensaver这样。

代码实现：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hex Clock</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans:wght@400;700&display=swap" rel="stylesheet">
    
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            font-family: 'Noto Sans', sans-serif;
            transition: background-color 1s;
            color: white;
            text-align: center;
        }
        #time {
            font-size: 5em;
            margin: 0;
        }
        #hex {
            font-size: 2em;
            margin: 0;
            opacity: 0.7;
        }
    </style>
</head>
<body>
    <div>
        <div id="time">00:00:00</div>
        <div id="hex">#000000</div>
    </div>

    <script>
        function updateClock() {
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');
            
            const timeString = `${hours}:${minutes}:${seconds}`;
            const hexColor = `#${hours}${minutes}${seconds}`;

            document.getElementById('time').textContent = timeString;
            document.getElementById('hex').textContent = hexColor;

            document.body.style.backgroundColor = hexColor;
        }

        setInterval(updateClock, 1000);
        updateClock();
    </script>
</body>
</html>
```
保存到[网站](http://you7n.com/hex-clock/)上，输入进屏保选项里就可以了——当然也可以使用本地文件 但是似乎感觉不太好使。

以上。
