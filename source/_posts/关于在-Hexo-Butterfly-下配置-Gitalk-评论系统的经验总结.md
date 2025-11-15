---
title: 关于在 Hexo Butterfly 下配置 Gitalk 评论系统的经验总结
date: 2025-07-09 03:08:47
tags: GitHub Pages
categories:
  - 踩坑记录
  - 经验总结
---

刚开始配置 Gitalk 的时候，虽然对我来说配置并不是什么难事，但因为踩坑而出现评论区不同步的 bug 而放弃了一阵子，属实可惜。

当时一度以为这是 Gitalk 本身的 bug。**经过一番研究，才知道我错了。**

**Butterfly 主题已经自带了 Gitalk 插件，不需要通过 Hexo 安装**。如果之前已经做好了 Gitalk 配置的一切准备工作，只要在 `_config.butterfly.xml` 配置文件的 `Gitalk` 部分填上对应的 `cilent_id`、`client_secret`、`repo`、`owner`、`admin`，然后 `option` 处根据自己喜好配置。

一切准备就绪后，保存文件，并在 Git Bash 里执行「hexo 三连」命令 `hexo clean && hexo generate && hexo deploy`，就完成了。**如果没问题的话，用不同设备打开评论区，就不会再出现评论区不同步和 Issues 重复的问题。**

## 如果之前通过 Hexo 命令安装过 Gitalk，该怎么办？

执行以下命令，删除 `hexo-plugin-talk` 插件。

``` Git Bash
npm uninstall hexo-plugin-gitalk
```