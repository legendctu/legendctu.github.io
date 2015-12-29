---
layout: post
title: CSV文件输出
categories: [php]
---

excel打开utf-8编码的csv会乱码，wps则支持gbk、utf-8编码的csv。输出时需要把内容转码为gbk。
另，保存文件名，建议先url encode。这样ie可支持中文等字符。文件名则不一定要转换为gbk编码。以utf-8输出文件名，ie8、chrome实测通过。

Example written in PHP:

```php
<?php
    header('Content-Type: text/csv; charset=gbk');
    header('Content-Disposition: attachment; filename=' . urlencode('我是中文.csv'));
    echo iconv('utf-8', 'gbk', '我是表头,我还是表头'), "\n";
    exit;
```
