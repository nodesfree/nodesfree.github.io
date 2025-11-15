---
title: 如何正确给 Windows 8 原版镜像离线安装更新补丁
date: 2025-07-30 09:44:55
tags: Windows
categories: 教程
---

{% note warning flat %}
本文针对的是 Windows 8 (NT 6.2, build 9200)，并非 Windows 8.1 (NT 6.3, build 9600)！
{% endnote %}

## 前言

众所周知，微软官方并不像现在的 Windows 10/11 那样提供带更新的镜像（LTSB/LTSC 除外）。而我们直接安装目标系统，在目标系统里安装更新补丁很费时且麻烦，还有失败的可能。

微软早在 2016 年 1 月 12 日就已停止对 Windows 8 的支持，而同为 NT 6.2 内核的 Windows Embedded 8 直到 2023 年 7 月 11 日才停止 ESU 支持，Windows Server 2012 则是在 2026 年 10 月停止 ESU 支持。但同为 NT 6.2 内核的 Windows Server 2012 只有 64 位而没 32 位，因此 32 位的 Windows 8 和 Windows Embedded 8 的更新补丁只能安装到 2023 年 7 月 11 日，而 64 位 Windows 8、Windows Embedded 8 和 Windows Server 2012 则可以在 2026 年 10 月之内继续安装更新。

为此，我们就要挂载镜像，为 Windows 8 离线安装更新补丁和 BypassESU。

---

## 所需内容

- [Windows 8 原版镜像](https://files.rg-adguard.net/language/4eba0b22-2762-b54c-e07b-9853fe39a726)
- [7-Zip](https://www.7-zip.org/) 或其他解压缩软件
- [UltraISO](https://www.423down.com/1397.html)
- [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language)
- [NTLite](https://www.ntlite.com/)
- [BypassESU-Blue-v2](https://gitlab.com/stdout12/adns/uploads/ef7c376b2a039ff69ce94ee5fd8c445d/BypassESU-Blue-v2.7z)
- [WSUS Offline Update](https://github.com/Dora-Honor/wsus-offline-legacy/releases/download/1032/wsusoffline1032.7z) ([Gitee](https://gitee.com/Dora-Honor/wsus-offline-legacy/releases/download/1032/wsusoffline1032.7z))
- [Windows 8 安装 IE 11 和 .NET 4 先决更新补丁](https://gitlab.com/stdout12/adns/uploads/c7c9f583da309adfb5f7a621ff3cf218/W8_NetFx4_IE11_Prereqs.7z)
- [.NET Framework 4 安装工具](https://gitlab.com/stdout12/adns/uploads/f4f25dddf99ae700e0ae0007473171f3/dotNetFx48-W8.zip)
- 由 WSUS Offline Update 下载的更新补丁
- [用于 NT 6.2 的 IE 11 更新补丁和语言包](https://www.catalog.update.microsoft.com/Search.aspx?q=kb4492872)
- [用于 NT 6.2 的 .NET Framework 4.8 更新补丁](https://www.catalog.update.microsoft.com/Search.aspx?q=kb4486081)和[语言包](https://www.catalog.update.microsoft.com/Search.aspx?q=kb4087513)
- [VMware Workstation Pro 虚拟机](https://www.ghxi.com/vmware17.html)
- **丰富的计算机知识**
- **充分的理解能力**

---

## 正文

### 解压 ISO

- 使用 7-Zip 或者其他解压缩软件将 ISO 解压到合适的目录。
- **最好选择【解压到 \\*】选项。**

### 启用 .NET Framework 3.5 和安装 WE8 SKU 补丁

- 启动 Dism++，在主界面选择【文件】—【挂载映像】，填入 WIM 文件路径和挂载目录。

{% note warning %}
注意挂载目录要先创建，**不能是根目录、有文件的目录、非半角英文数字字符目录！**\
{% endnote %}

![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/01.png)  
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/02.png)

#### 启用 .NET Framework 3.5

1. 打开会话，选择【程序和功能】—【Windows 功能】，查看是否有本地源。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/03.png)
2. 如有则双击【.NET Framework 3.5 (包括 .NET 2.0 和 3.0)】复选框，将其变成填充状态以启用 .NET Framework 3.5。
3. 选择【应用】，等待一段时间后就启用了。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/04.png)

{% note info flat %}
保险起见，最好先保存映像，避免因后续失败而花大量时间重做。
{% endnote %}

#### 安装 WE8 SKU、IE 11、.NET 4 更新补丁

{% note info warning %}
注意一定要按照安装顺序进行，**否则会出现「不适合」的现象而无法安装！**
{% endnote %}

1. 解压 [Windows 8 安装 IE 11 和 .NET 4 先决更新补丁](https://gitlab.com/stdout12/adns/uploads/c7c9f583da309adfb5f7a621ff3cf218/W8_NetFx4_IE11_Prereqs.7z)，里面有 `x64` 和 `x86` 文件夹，分别代表 64 位和 32 位，根据镜像位宽选择即可。以下以 64 位为例。
2. 在 Dism++ 选择【更新管理】—【Windows Update】—【添加】，找到先决更新补丁所在位置，进入 `x64` 文件夹，先选中以 `Microsoft-Windows-Embedded-SKU` 开头的全部 `MUM` 文件，然后单击【打开】安装 SKU 更新，等待一段时间。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/05.png)  
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/06.png)
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/07.png)
1. 重复上一个步骤，选中 `Microsoft-Windows-Embedded-IE-Desktop-Package~31bf3856ad364e35~amd64~~6.2.9200.16384.mum`、`Microsoft-Windows-Embedded-NetFx4Extended-Package~31bf3856ad364e35~amd64~~6.2.9200.16384.mum` 和以 `Microsoft-Windows-Embedded-NetFx4` 开头的更新补丁，单击【打开】安装 SKU 更新，等待一段时间。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/08.png)
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/09.png)

{% note info %}
保险起见，最好先保存映像，避免因后续失败而花大量时间重做。
{% endnote %}

### 安装 IE 11 和 .NET 4.8

#### 安装 IE 11

1. 重复上述安装步骤，找到 IE 11 及语言包更新补丁所在位置，先选择 `ie11-win6.2_8d823347334b5cb35a0d174a4267e03a8ae8ccc0.msu` 打开安装 IE 11，等待一段时间。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/10.png)
2. 然后选择 `ie11-windows6.2-languagepack-x64-*.msu` 语言包更新补丁，安装 IE 11 语言包，等待一段时间。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/11.png)

{% note info %}
保险起见，最好先保存映像，避免因后续失败而花大量时间重做。
{% endnote %}

#### 安装 .NET Framework 4.8

1. 解压安装器，将 .NET Framework 4.8 及语言包更新补丁放在和 .NET Framework 4 安装器同一个目录。
2. 右键单击 `NET48.cmd`，在弹出的快捷菜单中选择【以管理员身份运行】。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/12.png)
3. 输入所在的 WIM 挂载路径，按 Enter 执行安装。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/13.png)
4. 在 Dism++ 保存映像，然后卸载。

{% note info %}
保险起见，最好先保存映像，避免因后续失败而花大量时间重做。
{% endnote %}

### 使用 BypassESU-Blue-v2 安装 ESU 破解补丁

1. 解压 `BypassESU-Blue-v2.7z` 压缩包，找到所在位置，右键单击 `Wim-Integration.cmd`，在弹出的快捷菜单中选择【以管理员身份运行】。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/14.png)
2. 输入 WIM 映像所在路径，按 Enter 进入下一步。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/15.png)
3. 等待检测，按 <2> 进行安装。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/16.png)  
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/17.png)

### 使用 WSUS Offline Update 下载更新补丁

1. 解压 WSUS Offline 压缩包，右键单击 `UpdateGenerator.exe`，选择【以管理员身份运行】。
2. 选择 [Legacy products]，在 [Windows 8 / Server 2012] 处勾选 [x64 Global]，Option 处按照截图勾选。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/18.png)
3. 单击 [Start] 开始下载更新补丁。等待一段时间。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/19.png)

{% note info %}
更新补丁下载在程序目录的 `cilent\w62-x64\glb` 处。
{% endnote %}

### 使用 NTLite 安装更新补丁

1. 打开 NTLite，将 WIM 拖进窗口，双击分卷挂载 WIM。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/20.png)
2. 找到补丁的下载位置，选中除 `ie11-*.cab` 和 `windows8-rt-kb2966827-x64_6d16009f8bdb735822cfb385d3ecf2c7b74a567a.cab` 外的所有更新补丁包，将其拖入 NTLite 的【更新整合】处，等待分析一段时间。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/21.png)  
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/22.png)
3. 完成后，选择【结束】下的【应用】，再单击【工具栏】选项卡下的【开始】，确认应用更改。
  ![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/23.png)

{% note info %}
更新补丁安装需要等待一段时间。
{% endnote %}

{% note danger %}
一定不要安装 `KB2966827` 更新补丁，否则 **.NET Framework 3.5 会被关闭，且无法再启用！**
{% endnote %}

---

## 在虚拟机或实体机安装测试

- 此步略。

![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/24.png)  
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/25.png)  
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/26.png)  
![](https://raw.githubusercontent.com/Dora-Honor/mskk-blog/refs/heads/main/Images/250730/27.png)

---

## 微软对 Windows 8 安装 IE 11 和 .NET 4 设置阻碍的说明

- 在 Windows 8 上，**无法直接安装 IE 11 和 .NET 4，在未安装 SKU 先决更新补丁的情况下均会报错**。而同为 NT 6.2 内核的 Windows Embedded 8 和 Windows Server 2012 则不受影响。
- 就算装上 SKU 先决更新补丁，从微软官网下载的 .NET 4 安装包**运行后也会报错**。因此需要用 Windows Update 的更新补丁包进行安装。

---

## 参考资料

- abbodi1406's Batch Scripts Repo | Page 115 | My Digital Life Forums. https://forums.mydigitallife.net/threads/abbodi1406s-batch-scripts-repo.74197/page-115#post-1775602. 2023-02-04
- Windows 8 x86恢复Windows更新功能并安装WES8更新指南 - 哔哩哔哩. https://www.bilibili.com/opus/926499762345082896. 2024-05-01
- 2020 年 1 月微软对 Windows 7 停止支持后，有没有什么延寿的方法？ - 知乎. https://www.zhihu.com/question/352300826/answer/870744486. 2020-01-14
