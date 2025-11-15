---
title: 关于 sunflower2333 大佬的 MIX 2s (polaris) 音频驱动安装和使用问题的总结
date: 2025-04-22
tags: 踩坑记录
categories: 踩坑记录
---

# [sunflower2333 大佬的 MIX 2s (polaris) 音频驱动](https://github.com/sunflower2333/mix2s_tas2559_2560_win)

实测在 IoT LTSC 2021 (21H2, 19044) 和 22H2 (19045) 等 1904x 系版本，以及 IoT LTSC 2024 (24H2, 26100) 上用 DriverUpdater 和 Mindows 工具箱离线导入驱动后，系统会蓝屏两次，两次重启之后驱动就正常了。

个人建议在安装配置前，先不要导入音频驱动，部署好重启后再在大容量模式下用 Mindows 工具箱导入驱动。因为这样蓝屏两次不会丢数据，也不会损坏完整性。
