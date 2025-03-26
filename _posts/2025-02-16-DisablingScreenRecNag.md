---
layout: post
title: 【已解决】消除macOS 15.2以上版本频繁录屏提示
date: 2025-02-12
tags: [macOS, dev]
author: you7n
---

通过运行Bash脚本的方式解决这个烦人的提示。
{:message}

自从升级到 macOS 15.3 以后，屏幕右上角就总有像这样的提示，非常分散注意力，影响工作效率。

<img width="319" alt="image" src="https://github.com/user-attachments/assets/97f58815-e35c-4af1-9391-0129ba7bae3f" />

<img width="303" alt="image" src="https://github.com/user-attachments/assets/4f4b8b78-6187-4824-b8d9-56e0ee12337e" />

原因是从 macOS 15.2 开始，安全策略升级了。除此之外也会有录屏/截屏软件每个月都要允许访问这样的操作，参见[(这里应该有一个链接但是还没写完相关内容)]()。

## 解决方法

这个烦人的提示的解决办法也很简单，命令行一行就能解决。

```shell
curl -L "https://github.com/luckman212/screencapture-nag-remover/releases/download/1.3.3/screencapture-nag-remover.sh" | bash
```

## 工作原理

该脚本修改 macOS 内部的 ScreenCaptureApprovals.plist 文件，并提供了自动化方法，使其在后台持续运行。

### 1. 检测 macOS 版本

脚本首先通过 sw_vers --productVersion 解析 macOS 版本号，并检查系统是否为 macOS 15 或更高版本。如果版本低于 15，则脚本直接退出。
```
IFS='.' read -r MAJ MIN _ < <(/usr/bin/sw_vers --productVersion)
if (( MAJ < 15 )); then
    echo >&2 "this tool requires macOS 15 (Sequoia)"
    exit
fi
```
### 2. 目标文件与关键路径

脚本涉及的核心系统文件包括：

- ScreenCaptureApprovals.plist（管理应用程序的屏幕截图许可）

- TCC.db（TCC，即 Transparency, Consent, and Control 数据库）

- LaunchAgents 目录（用于自动执行后台任务）

其中，ScreenCaptureApprovals.plist 记录了应用程序何时最后请求过屏幕截图权限，脚本的核心原理是修改此文件，使 macOS 认为所有应用程序的警告时间被推迟到 100 年后。

```
FUTURE=$(/bin/date -j -v+100y +"%Y-%m-%d %H:%M:%S +0000")
```

### 3. 修改 ScreenCaptureApprovals.plist

如果 macOS 版本高于 15.1，则脚本会使用 defaults write 命令，将 kScreenCaptureApprovalLastAlerted 和 kScreenCaptureApprovalLastUsed 设置为未来 100 年后的时间。

```
/usr/bin/defaults write "$PLIST" "$1" -dict \
    kScreenCaptureApprovalLastAlerted -date "$FUTURE" \
    kScreenCaptureApprovalLastUsed -date "$FUTURE"
```

### 4. 生成 MDM 配置文件（适用于 15.1+）

macOS 15.1 及以上版本提供了一个官方方法，通过 MDM（移动设备管理）策略来禁止所有应用程序的截图警告。脚本可以自动生成 MDM 配置文件 macOS_15.1_DisableScreenCaptureAlerts.mobileconfig，用户可以手动导入到 MDM 服务器。

```
<key>forceBypassScreenCaptureAlert</key>
<true/>
```

这使得所有受 MDM 管理的 macOS 设备不会再显示截图警告。

### 5. 创建 LaunchAgent 以定期运行

为了让该脚本定期执行，脚本会创建 LaunchAgent 配置文件，使其每 24 小时运行一次。

```
<key>StartInterval</key>
<integer>86400</integer>
```

并使用 launchctl 命令注册该任务，使其在后台运行。

```
/bin/launchctl bootstrap gui/$UID "$AGENT_PLIST"
```

### 6. 提示用户授予完全磁盘访问权限

由于 ScreenCaptureApprovals.plist 和 TCC.db 都受系统保护，脚本需要 Full Disk Access（完全磁盘访问权限）。如果脚本检测到 bash 没有该权限，它会引导用户手动授予。

```
/usr/bin/open 'x-apple.systempreferences:com.apple.preference.security?Privacy_AllFiles'
```


