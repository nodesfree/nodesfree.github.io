---
title: 关于在 Windows 8 上加载 WofAdk 驱动重启后卡修复的解决方法
date: 2025-07-26 21:32:11
tags: Windows
categories: 踩坑
---

> 该内容与 Windows 8 (NT 6.2, build 9200) 相关，并非 Windows 8.1 (NT 6.3, build 9600)。  

最近，在折腾 Windows 8 (NT 6.2, build 9200) 系统的续命补丁和 IE 11 的安装，这些整体还是很顺利。  
但最近遇到了一个棘手的事情，那就是在系统里用 [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language)，在开了【加载 WofAdk 驱动】的情况下优化和清理，重启后就会卡修复。

![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250727/1.png)  
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250727/2.png)

经过排查和日志信息，发现是 `wofadk.sys` 驱动的版本太高而未能正常加载。将其替换成 10.0.14393.x 及以下版本即可解决。

![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250727/3.png)  
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250727/4.png)