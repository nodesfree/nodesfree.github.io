---
title: 关于如何在 Xiaomi Mi 9T Pro / Redmi K20 Pro (raphael) 上重装 Win11 24H2 (26100) 及以上版本系统（用工具箱分配了分区的情况下）
date: 2025-04-15
updated:
tags: 踩坑记录
categories: 踩坑记录
keywords:
description:
top_img:
comments:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---

# 所需工具

* [Mindows 工具箱](https://mindows.cn)
* [Dism++](https://github.com/Chuyu-Team/Dism-Multi-language)
* WIM、ESD 或 SWM 镜像
* Xiaomi Mi 9T Pro / Redmi K20 Pro (raphael) 一台
* 稳定的数据线、USB 接口
* 丰富的计算机知识

---

# 正文

最近想在 Xiaomi Mi 9T Pro / Redmi K20 Pro (raphael) 上，通过 Mindows 工具箱安装 Windows 11 IoT 企业版 LTSC 2024，**突然发现 Mindows 工具箱还不支持 24H2 及以上版本 WIM，选择之后会报错**。如图所示：
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250415/250415-1.png)

那么我是如何解决的？首先按住音量下键和电源键进入 Fastboot 模式连接电脑，然后在工具箱主界面选择【进入大容量模式】，再在工具箱中选择第 1 项，使用 Fastboot boot 临时进入大容量挂载模式。

![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250415/250415-2.png)

一切就绪之后，先进入文件资源管理器查看系统盘是否有盘符，如果有，则使用 Dism++ 释放到该分区。

![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250415/250415-3.png)
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250415/250415-4.png)
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250415/250415-5.png)

提示「还原成功」后，回到工具箱，分别进行【修复系统引导】【安装、删除驱动】【跳过开机向导】即可。
