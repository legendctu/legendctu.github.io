---
layout: post
title: Kill掉Processlist的进程
categories: [mysql]
---

## 通过mysqladmin操作

```bash
./mysqladmin -h host -P port -u username -p
```

```sql
USE information_schema;
SELECT * FROM PROCESSLIST LIMIT 0, 50; --偏移量自行定义
KILL 123; --123为进程id
```

或者

```bash
./mysqladmin -h host -P port -u username -p processlist
./mysqladmin -h host -P port -u username -p kill process_id
```

