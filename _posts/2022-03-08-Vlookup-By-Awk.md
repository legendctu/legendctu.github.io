---
layout: post
title: Vlookup By Awk
categories: [memo, shell]
---

```bash
awk -F',' '{if(FNR==NR){arr[$1]=$2} if(FNR!=NR){print $1","arr[$1]}}' haystack.file target.file
```

FNR refers to the record number (typically the line number) in the current file.
NR refers to the total record number.

Load the haystack first. Then print the value of your target.


例子：播放量匹配

```sh
awk -F',' '{if(FNR==NR){arr[$1]=$2} if(FNR!=NR){print $1","arr[$1]}}' songid.playcount.txt songid.list.csv
```

