---
layout: post
title: Fix Wsl Missing System Vhd
categories: [memo, shell]
---

```
无法将磁盘“C:\Program Files\WSL\system.vhd”附加到 WSL2： 系统找不到指定的文件。
错误代码: Wsl/Service/CreateInstance/CreateVm/MountVhd/HCS/ERROR_FILE_NOT_FOUND
```

Solution:

1. Download msi from Microsoft/WSL at Github (https://github.com/microsoft/WSL/releases)
2. Execute the following command to extract system.vhd from the msi file
3. Copy the vhd file to the path mentioned above

```
msiexec /a F:\wsl.2.3.24.0.x64.msi /qb TARGETDIR=f:\tmp
```
