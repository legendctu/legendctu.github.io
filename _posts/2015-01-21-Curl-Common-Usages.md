---
layout: post
title: Curl Common Usages
categories: [memo, shell]
---

2015-12-29 updated:
A simple way to fetch contents using host:

```bash
curl -H 'Host: legendctu.github.io' 'http://127.0.0.1/shell'
```

2015-01-21:

```bash
curl --user-agent 'this is ua' --cookie 'this=cookie&key=value' -I -x host:port 'http://legendctu.github.io'
```

**-I**

Show response header only.


**-x**

Use proxy, acting as using a host.


PS:

  Here's a function using curl, written in PHP. Hosts are optional.

```php
<?php
/**
 *  @brief 根据HOST获取内容 by BenjaminLeung 2014.03.28
 *  
 *  @param [in] $posturl  请求链接
 *  @param [in] $postvars post data
 *  @param [in] $timeout  请求超时，单位秒
 *  @return 抓取内容
 */
function _curlHostRequest($posturl, $postvars = null, $timeout = 3, $host = '') {
    $ch = curl_init($posturl);
    if (!empty($postvars)){
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_POSTFIELDS, $postvars);
    }
    curl_setopt($ch, CURLOPT_TIMEOUT, $timeout);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_DNS_USE_GLOBAL_CACHE, false);

    $cookie = '';
    if(!empty($_COOKIE)){
        foreach($_COOKIE as $k => $v){
            $cookie .= "{$k}={$v}; ";
        }
        $cookie = substr($cookie, 0, -2);
    }

    curl_setopt($ch, CURLOPT_COOKIE, $cookie);

    if(!empty($host)){
        curl_setopt($ch, CURLOPT_HTTPHEADER, array("Host: {$host}"));
    }

    $Rec_Data = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    if($httpCode !== 200){
        $Rec_Data = false;
    }
    curl_close($ch);
    return $Rec_Data;
}

_curlHostRequest('http://10.13.0.33/sample', http_build_query($arr), 3, 'legendctu.github.io');
```

