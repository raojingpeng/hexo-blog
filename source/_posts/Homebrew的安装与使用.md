---
title: Homebrew的安装与使用
author: Ackerman
date: 2021-12-06 23:16:43
categories:
- Mac工具集
---

### Homebrew是什么

Homebrew是一款Mac OS平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。
<!--more-->

### Homebrew组成

| #                | 功能                                |
| ---------------- | ----------------------------------- |
| Homebrew         | 源代码仓库                          |
| Homebrew-core    | homebrew 核心源                     |
| Homebrew-cake    | 提供macos应用和大型二进制文件的安装 |
| Homebrew-bottles | 预编译二进制软件包                  |

### Homebrew安装

```shell
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install.sh)"
```

1. 脚本内置[中科大镜像](https://mirrors.ustc.edu.cn/help/brew.git.html) ，所以能让Homebrew安装的更快。

2. 脚本将 Homebrew, Homebrew-core 安装源替换为了中科大源，提升国内软件安装速度

### 基本用法

```shell
// 查询
brew search 软件名

// 安装
brew install 软件名

// 卸载
brew uninstall 软件名

// 更新 Homebrew
brew update 

// 查看 Homebrew 配置信息
brew config 
```

