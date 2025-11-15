---
title: 离线更改目标 Windows SKU
date: 2025-07-27 23:01:45
tags: Windows
categories: 教程
---

## 使用注意

以下命令**均要以管理员身份在 `cmd` 或 `PowerShell` 中运行。**

## Windows 8 (NT 6.2, build 9200)

对于 Windows 8，微软官方尚未提供镜像，因此需要用命令行转换 SKU。

{% note info %}
**必须用 Retail 零售版镜像的专业版，不能用 VL 批量授权版镜像。VL 批量授权版不支持添加 Windows 功能！**
{% endnote %}

首先要挂载目标镜像，输入以下命令（也可使用 [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language) 挂载）：

``` cmd
dism /Mount-Image /ImageFile:X:\xxx\xxx.wim /Index:x /MountDir:X:\xxxx
:: 「X」处需自行替换成自己镜像的对应目录。
```

挂载完后，执行 `dism /Image:X:\xxxx /Get-TargetEditions` 命令，获取目标 SKU。

``` cmd
dism /Image:X:\xxxx /Get-TargetEditions
:: 「X」处需自行替换成自己镜像的对应目录。
```

然后执行 `dism /Image:X:\xxxx /Set-TargetEdition:ProfessionalWMC` 命令，将专业版转换为带 WMC 的专业版。

``` cmd
dism /Image:X:\xxxx /Set-TargetEdition:ProfessionalWMC
:: 「X」处需自行替换成自己镜像的对应目录。
```

最后别忘记保存并卸载镜像，完成操作。
> 在 [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language) 则要先保存（选择【直接保存】）后再卸载。

``` cmd
dism /Unmount-Image /MountDir:X:\xxxx /commit
:: 「X」处需自行替换成自己镜像的对应目录。最后的 /commit 保存更改命令不要遗漏。
```

## Windows 8.1 (NT 6.3, build 9600)

- 对于 Windows 8.1，在 WSUS 和 [基于 Microsoft® 列出的文件列表](https://files.rg-adguard.net/language/138dda8e-bacd-0d47-cc74-af23f9f489a0) 上已有专业版（含 Media Center）ESD 镜像，只需用 [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language) 解密 ESD 而无需转换 SKU。
- 当然，Windows 8.1 同样支持 SKU 转换。

{% note info %}
**必须用 Retail 零售版镜像的专业版，不能用 VL 批量授权版镜像。VL 批量授权版不支持添加 Windows 功能！**
{% endnote %}

首先要挂载目标镜像，输入以下命令（也可使用 [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language) 挂载）：

``` cmd
dism /Mount-Image /ImageFile:X:\xxx\xxx.wim /Index:x /MountDir:X:\xxxx
:: 「X」处需自行替换成自己镜像的对应目录。
```

挂载完后，执行 `dism /Image:X:\xxxx /Get-TargetEditions` 命令，获取目标 SKU。

``` cmd
dism /Image:X:\xxxx /Get-TargetEditions
:: 「X」处需自行替换成自己镜像的对应目录。
```

然后执行 `dism /Image:X:\xxxx /Set-TargetEdition:ProfessionalWMC` 命令，将专业版转换为带 WMC 的专业版。

``` cmd
dism /Image:X:\xxxx /Set-TargetEdition:ProfessionalWMC
:: 「X」处需自行替换成自己镜像的对应目录。
```

最后别忘记保存并卸载镜像，完成操作。
> 在 [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language) 则要先保存（选择【直接保存】）后再卸载。

``` cmd
dism /Unmount-Image /MountDir:X:\xxx /commit
:: 「X」处需自行替换成自己镜像的对应目录。最后的 /commit 保存更改命令不要遗漏。
```

## Windows 10 IoT 企业版 LTSC 2021、Windows 11 IoT 企业版 LTSC 2024 及以后版本

- 对于 LTSC 2021 及以上版本，非 IoT 版仅有 5 年支持，而 IoT 版有 10 年支持。
- 无需转换 SKU 的版本仅有英文镜像。

首先要挂载目标镜像，输入以下命令（也可使用 [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language) 挂载）：

``` cmd
dism /Mount-Image /ImageFile:X:\xxx\xxx.wim /Index:x /MountDir:X:\xxxx
:: 「X」处需自行替换成自己镜像的对应目录。
```

挂载完后，执行 `dism /Image:X:\xxxx /Get-TargetEditions` 命令，获取目标 SKU。

``` cmd
dism /Image:X:\xxxx /Get-TargetEditions
:: 「X」处需自行替换成自己镜像的对应目录。
```

然后执行 `dism /Image:X:\xxxx /Set-TargetEdition:IoTEnterpriseS` 命令，转换为带 IoT 的 LTSC 版。

``` cmd
dism /Image:X:\xxxx /Set-TargetEdition:IoTEnterpriseS
:: 「X」处需自行替换成自己镜像的对应目录。
```

最后别忘记保存并卸载镜像，完成操作。
> 在 [Dism++](https://github.com/Chuyu-Team/Dism-Multi-Language) 则要先保存（选择【直接保存】）后再卸载。