---
title: "Chocolatey"  
date: "2020-05-17 4:06:19 -0700"
template: "post"
draft: true
slug: "chocolatey"
category: "other"
description: "Chocolatey"
---

[官方文档](https://chocolatey.org/install)

1. 打开一个Administrative shell (WIN + X，选择Command Prompt(Admin))

2. 使用Powershell执行如下命令：

   ```po
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ```

3. 安装成功！使用`choco` 或者`choco-?` 可以查看用法

注意：在使用choco安装的时候都应该使用Admin状态下的命令行，否则会有如下提示：

> Chocolatey detected you are not running from an elevated command shell
>  (cmd/powershell).
>
>  You may experience errors - many functions/packages
>  require admin rights. Only advanced users should run choco w/out an
>  elevated shell. When you open the command shell, you should ensure
>  that you do so with "Run as Administrator" selected. If you are
>  attempting to use Chocolatey in a non-administrator setting, you
>  must select a different location other than the default install
>  location. See
>  https://chocolatey.org/install#non-administrative-install for details.

## 常用命令

`choco install [Package Name]` 安装包

之后可以使用`refreshenv`来刷新环境变量

