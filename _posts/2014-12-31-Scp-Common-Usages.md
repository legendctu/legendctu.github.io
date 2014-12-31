---
layout: post
title: Scp Common Usages
categories: [memo, shell]
---

```shell
scp -P port source target
```


本地     拷贝到     远程

```shell
scp -P port /path/to/file remote_username@remote_ip:/path/to/file
```


远程     拷贝到     本地

```shell
scp -P port remote_username@remote_ip:/path/to/file /path/to/file
```


