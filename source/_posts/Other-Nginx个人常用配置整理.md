---
title: '[Other-nginx个人常用配置整理]'
date: 2022-06-24 11:06:43
categories: Other
tags: nginx
---

`
nginx -t  检查配置是否正确 
`

`
nginx -s reload 重新加载配置信息
`

```
server {
    listen 80;
    server_name www.xxx.com
    location / {
    alias /path/;
}
```

```
server {
    listen 80;
    server_name www.xxx.com
    location / {
    proxy_pass http://127.0.0.1:8088
}
```

```
# HTTPS
server {
    listen 443 ssl;
    index index.html index.htm index.php;
    server_name www.xxx.com;

    ssl_certificate /xxxxxx.pem;
    ssl_certificate_key /xxxxxx.key;

    ssl_session_timeout 5m;

    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
    proxy_pass http://127.0.0.1:8088;
   }
}
```

```
# ios Swift 通用链接(Universal Links)
location / {
    charset UTF-8;
    default_type text/html;
    return 200 '{"applinks":{"apps":[],"details":[{"appID":"xxxxxxxxxx","paths":["*"]}]}}';
    }
}
```

```
#重定向
server {
      listen 80;
      server_name www.xxx.com;
      rewrite ^(.*)$ https://$host$1; #将所有HTTP请求通过rewrite指令重定向到HTTPS。
      location / {
        proxy_pass http://127.0.0.1:8088;
    }
}
```
