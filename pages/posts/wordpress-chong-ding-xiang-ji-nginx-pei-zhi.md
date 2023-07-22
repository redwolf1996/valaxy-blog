---
title: 'wordpress重定向及nginx配置'
date: 2020-10-30 16:34:02
tags: [服务端]
published: true
hideInList: false
feature: 
isTop: false
---
### wordpress重定向
```
location / {
    index index.html index.php;
    if (-f $request_filename/index.html){
        rewrite (.*) $1/index.html break;
    }

    if (-f $request_filename/index.php){
        rewrite (.*) $1/index.php;
    }

    if (!-f $request_filename){
        rewrite (.*) /index.php;
    }
}
rewrite /wp-admin$ $scheme://$host$uri/ permanent;
```

### https配置
- 1. 在阿里云或其他地方申请证书文件得到如：
214040226730432.key  214040226730432.pem
放到如  /etc/nginx/cert目录下

- 2.nginx配置文件里面
```
server {
    listen 443;
    server_name jadewen.win www.jadewen.win;
    ssl on;
    root /var/www/default;
    index index.php index.html;
    ssl_certificate  cert/214040226730432.pem;
    ssl_certificate_key  cert/214040226730432.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
      try_files $uri $uri/ /index.php?$query_string;
    }

      location ~ \.php$ {
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
}
```

- 3.重定向http到https
```
server {
    listen        80;
    server_name  jadewen.win www.jadewen.win;
    return 301    https://$host$request_uri;
}
```