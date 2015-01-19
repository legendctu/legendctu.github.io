---
layout: post
title: Kill掉Processlist的进程
categories: [memo, mysql]
---

### 1. 通过mysql操作

```mysql
USE information_schema;
SELECT * FROM PROCESSLIST LIMIT 0, 50; --偏移量自行定义
KILL 123; --123为进程id
```

### 2. 通过mysqladmin操作

```bash
./mysqladmin -h host -P port -u username -p processlist
./mysqladmin -h host -P port -u username -p kill process_id
```


