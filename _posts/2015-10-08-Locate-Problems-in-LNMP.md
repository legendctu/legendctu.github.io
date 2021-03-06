---
layout: post
title: 在LNMP环境下定位常见问题
categories: [experience]
---

## 适用环境
1. linux
2. nginx
3. php
4. mysql
5. redis

定位常见问题，并不针对特定环境，思路正确即可。仅是本文所使用工具暂时只支持linux + nginx。


## 基础部署
1. 对于linux + nginx环境，建议部署[nginx访问日志分析监控脚本](https://github.com/legendctu/nginx_access_log_monitor)，实时了解业务运行状况。目前仅对CentOS6.3兼容，支持请求响应状态分析、请求响应时长分析。
2. php-fpm开启slowlog记录（request_slowlog_timeout），用于分析造成慢请求的backtrace。


## 定位问题
1. 分析请求响应状态
  1. 若使用了 *基础部署* **1.** 中提及到的监控脚本，能迅速定位到发生异常或即将发生异常的具体业务及其应用服务器，甚至是具体业务接口。定位精确粒度视监控脚本具体设置而定。
  2. 若未使用，则手工对请求访问日志进行分析，观察响应状态异常部分。
  响应异常一般有以下几种情况：
    1. 引用不存在文件，或者重定向规则命中失败，导致发生404。
    2. 上游（如php）在达到nginx设置的请求超时值时未返回（未作出响应），导致发生499。
    3. 上游（如php）有代码错误，导致发生500。
    4. 上游（如php）挂死，导致发生502。
    5. 上游响应虽是200，但响应时长大幅度上涨。

2. 初步处理
若仅仅是上游挂死，重启即可恢复业务。恢复业务后再进行故障原因分析（详见下文）。
404、500响应则修正配置、修正代码即可。

3. 异常原因初步分析
分析php-fpm记录的slowlog，可得到具体耗时长的调用函数。此时基本可以定位到故障原因。
需要注意的是，对于情景 **5. 上游响应虽是200，但响应时长大幅度上涨** 有可能是请求普遍变慢，但又未达到slowlog记录超时阈值，此时slowlog并不能记录到耗时长的调用函数，接下来就需要分析异常诱因。

4. 异常诱因分析
  1. 观察应用服务器运行状态，常用工具 *top* （其常用用法撰写中），观察占用cpu、占用I/O的进程，观察是否有异常。最常见诱因是cpu占用过高、I/O占用过高，导致php濒临崩溃，服务器负载升高。
  2. 观察mysql的process_list，观察是否有慢查询。常见诱因为mysql慢查询，导致php迟迟得不到结果，致使响应变慢，甚至挂死。
  3. 观察redis当前使用内存，是否已超过服务器可使用内存。当超过时，redis开始使用swap，此时因使用I/O而使响应变得异常缓慢。
  4. 使用运维部署的监控系统（如我司所使用的KMC），观察出现异常的应用服务器及相关的redis、数据库服务器状态。常见观察指标有：服务器负载变化、网卡流量变化、I/O变化、redis/mysql相关操作变化（如qps等）。


## 总结
常见问题一般为：
1. 人为配置、功能代码有误。
2. php或redis或mysql服务器出现异常，常见诱因可能是流量上涨（暴涨）引起php挂死、内存不足引起redis响应慢、db慢查询多。

