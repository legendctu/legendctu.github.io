---
layout: post
title: Kill掉Processlist的进程
categories: [memo, mysql]
---

1. 获取processlist以得到进程id
  1. 通过在数据库 information_schema 对表PROCESSLIST查询得到：

```mysql
USE information_schema;
SELECT * FROM PROCESSLIST LIMIT 0, 50;
```

  2. 通过mysqladmin获取processlist：

```bash
./mysqladmin -h host -P port -u username -p processlist
```

