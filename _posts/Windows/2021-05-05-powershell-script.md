---
layout: post
title: PowerShell执行脚本
date: 2021-05-05
categories: [windows]
tags: [windows,PowerShell]
excerpt: PowerShell script
---

PowerShell脚本执行策略:
- Restricted： 禁止运行任何脚本和配置文件。
- AllSigned ：可以运行脚本，但要求所有脚本和配置文件由可信发布者签名，包括在本地计算机上编写的脚本。
- RemoteSigned ：可以运行脚本，但要求从网络上下载的脚本和配置文件由可信发布者签名；       不要求对已经运行和已在本地计算机编写的脚本进行数字签名。
- Unrestricted ：可以运行未签名脚本。

<br/>
PowerShell默认的执行策略就是“Restricted”，禁止任何脚本的执行。【Get-ExecutionPolicy】命令不区分大小写，用于获得当前的执行策略。

```bash
PS D:\Projects> Get-ExecutionPolicy
Restricted
```
<br/>

使用【Set-ExecutionPolicy】命令设置/更改执行策略，选择“RemoteSigned”这个执行策略，这个策略既安全又可以执行本地编写的脚本.
```bash
PS C:\Windows\system32> Set-ExecutionPolicy RemoteSigned

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/
fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): y
```
