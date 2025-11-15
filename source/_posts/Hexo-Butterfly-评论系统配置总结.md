---
title: Hexo + Butterfly 评论系统配置总结
date: 2025-07-06 15:10:11
tags: GitHub Pages
categories: 踩坑记录
---

最近在用 Hexo 框架加 Butterfly 主题搭建自己的 GitHub Pages 博客，想给自己的博客加入评论系统。

最常用的评论系统有 [Gitalk](https://gitalk.github.io/) 和 [Giscus](https://giscus.app/zh-CN)。其中，前者主要依赖仓库的 [Issues](https://github.com/Dora-Honor/dora-honor.github.io/issues)，后者则依赖 [Discussions](https://github.com/Dora-Honor/dora-honor.github.io/discussions)。

我首先试了下 Gitalk。虽然配置对我来说并不是什么难事，但我在测试评论的时候遇到了以下问题：

- 在不同设备、账号上，同文章评论区内容不同步，且 Issues 话题重复。 ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250706/image_2025-07-06_15-27-01.png)

- 报错。

``` Git Bash
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
TypeError: Cannot read properties of undefined (reading 'gitalk')
    at Hexo.<anonymous> (D:\Documents\hexo-blog-butterfly\node_modules\hexo-plugin-gitalk\index.js:2:41)
    at Hexo.tryCatcher (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\util.js:16:23)
    at Hexo.<anonymous> (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\method.js:15:34)
    at D:\Documents\hexo-blog-butterfly\node_modules\hexo\dist\extend\filter.js:58:67
    at tryCatcher (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\util.js:16:23)
    at Object.gotValue (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\reduce.js:166:18)
    at Object.gotAccum (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\reduce.js:155:25)
    at Object.tryCatcher (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\util.js:16:23)
    at Promise._settlePromiseFromHandler (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\promise.js:547:31)
    at Promise._settlePromise (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\promise.js:604:18)
    at Promise._settlePromise0 (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\promise.js:649:10)
    at Promise._settlePromises (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\promise.js:729:18)
    at _drainQueueStep (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\async.js:93:12)
    at _drainQueue (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\async.js:86:9)
    at Async._drainQueues (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\async.js:102:5)
    at Async.drainQueues [as _onImmediate] (D:\Documents\hexo-blog-butterfly\node_modules\bluebird\js\release\async.js:15:14)
    at process.processImmediate (node:internal/timers:485:21)
```
虽然 Gitalk 界面美观，但还是因此放弃了。后续有时间再研究以下怎么搞吧。

然后我再试了下 Giscus。目前用下来，除了界面不如 Gitalk 美观，其他都还好，同文章评论区内容也能同步。

## 本人配置 Giscus 的总结
- 注册 GitHub 账号和创建仓库（这个不用多说了吧）。
- 安装 [Giscus App](https://github.com/apps/giscus)。
- 在账户设置中，在 [Integrations] 选择 [Applications]，找到 [giscus] 在 [Repository access] 中选择 [Only select repository]，并设置为所需仓库。完成后单击 [Save] 保存设置。 ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250706/image_2025-07-06_15-40-20.png)
- 进入**所需仓库**的设置，在 [General] - [Features] 处勾选 [Discussions]。保存设置。
- 刷新仓库页面，选择 [Discussions] 选项卡，单击 [Categories] 右边的笔（编辑）按钮，选择 [New category]，在 [Category name] 处输入 “Announcements”（不带引号），**[Discussion Format] 处一定要选择 [Annoucement]！**
- [单击这里](https://giscus.app/zh-CN)进入 Giscus 配置，在【仓库】处输入你的用户名和仓库（格式为 `username/repositories`），【页面 ↔️ discussion 映射关系】处保持默认的第一项或选择第二项，【Discussion 分类】选择刚刚保存的【Annoucement】，特性和主题根据自己喜好选择。
- 打开 `_config.butterfly.yml` 配置文件，找到 `# comments system` 项，在 `use` 处输入 `Giscus`（注意冒号后面要一个空格间隔），`text`、`lazyload`、`count`、`card_post_count` 则根据你自己的喜好来调整。
- 往下翻，找到 `# Giscus` 选项，在 `repo` 处填写对应的用户名和仓库（格式 `username/repositories`），`repo_id` 和 `category_id` 处填写 Giscus 生成的 ID，`light_theme` 和 `dark_theme` 项保持不动，`js` 和 `opition` 根据需求设置即可。
- **注意冒号后面要一个空格间隔，参数则不带引号！否则会出现评论发不出去的问题！**

# 参考资料
- hexo-butterfly主题-giscus评论系统设置 - 知乎. https://zhuanlan.zhihu.com/p/603658639
- Giscus 配置. https://giscus.app/zh-CN
- Butterfly 文檔(三) 主題配置 | Butterfly. https://butterfly.js.org/posts/4aa8abbe/#%E8%A9%95%E8%AB%96
