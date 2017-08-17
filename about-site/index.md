---
title: 本站特性
date: 2016-12-11 13:12:10
---

本站启用了更加安全的 HTTPS 协议，以 Nginx 作为主站的 Web Server，本站的 [博客](https://www.fanhaobai.com) 运行于 NodeJS 环境，本站的 [维基](https://wiki.fanhaobai.com) 运行于 PHP 环境，数据库采用开源的 MySQL ，内存缓存服务器采用 Redis 。

在这里列出本站主要涉及的技术。

# 更新说明
* 2017.08.12：修改 Nginx [配置](#Nginx配置)。
* 2017.08.11：增加使用 Elasticsearch，[见这里](http://es.fanhaobai.com)。
* 2017.06.17：使用 GoAccess 分析日志，[见这里](https://www.fanhaobai.com/go-access.html)。
* 2017.05.03：启用 Composer [私有镜像仓库](http://packagist.fanhaobai.com)。
* 2017.04.06：增加 Solr 支持，见 [Solr Admin](http://solr.fanhaobai.com)。
* 2017.03.01：博客迁移到 [Hexo](https://www.fanhaobai.com/2017/03/install-hexo.html)。
* 2017.02.10：将 Nginx 配置增加「[限制恶意IP访问](https://www.fanhaobai.com/2017/02/lock-ip.html)」。
* 2017.02.07：增加 Nginx 配置，防止其他「[域名恶意解析]()」 到本服务器。
* 2017.01.20：增加使用 MongoDB，运行于 Docker 环境，[见这里](https://www.fanhaobai.com/2017/01/mongo-docker-install.html)。
* 2017.01.18：增加使用 [Docker](https://www.fanhaobai.com/post/docker-install.html)。
* 2017.01.16：将 Nginx 配置增加 [防盗链]()。
* 2017.01.14：统一流量入口，将 Nginx 配置增加 [fanhaobai.com]() 重定向到 [www.fanhaobai.com](https://www.fanhaobai.com)。
* 2017.01.12：增加使用 [Robots](https://www.fanhaobai.com/2017/01/robots.html)，协议文件 [robots.txt](http://www.fanhaobai.com/robots.txt) ，网站地图文件  [XML格式](http://www.fanhaobai.com/sitemap.xml)。
* 2016.12.13：增加多少评论的用户头像，将 HTTP 代理为 HTTPS， [见这里](https://www.fanhaobai.com/2017/03/install-hexo.html#多说头像HTTPS代理)。
* 2016.12.11：将主站博客迁移到 [FireKylin](https://www.fanhaobai.com/2016/12/firekylin.html)。
* 2016.12.08：增加使用 [HTTPS](https://www.fanhaobai.com/2016/12/firekylin.html)。
* 2016.12.07：增加使用 [NodeJS ](https://www.fanhaobai.com/2016/12/nodejs-install.html)。

# 所用技术

<div class="row"><div class="col m3 s4">![](/about-site/index/O_tDRRtwdgwUJGleCTb-fJvl.png)</div><div class="col m3 s4">![](/about-site/index/1b8RAFgTPJ65AAY7vARIDYLa.jpg)</div><div class="col m3 s4">![](/about-site/index/oIvyC4BVfC358zcqI3_8kQua.png)</div><div class="col m3 s4">![](/about-site/index/9fPXD11M-qsaym50SNAkwFl_.png)</div><div class="col m3 s4">![](/about-site/index/vvZnp6Cdl4KDz02shzLvJsfN.png)</div><div class="col m3 s4">![](/about-site/index/C6ADglLNRxwYUdhfid1qzI1u.jpg)</div><div class="col m3 s4">![](/about-site/index/UtjFTjHOIYx92yaA6wsdh0GD.png)</div><div class="col m3 s4">![](/about-site/index/fQm_6CQKLTPRYXQ_vPInMijE.jpg)</div><div class="col m3 s4">![](/about-site/index/ds31b8RAFgTPJ65AAY7esGe2.jpg)</div><div class="col m3 s4">![](/about-site/index/0f32aeff3ccf8e5dc3d196ee8fd4.png)</div></div>

# 配置信息

## Nginx配置

### 主配置——nginx.conf

```Nginx
user www www;
worker_processes  4;

error_log  /data/logs/error.log;
#pid        logs/nginx.pid;
#worker_rlimit_nofile 655350;

events {
    use epoll;
    worker_connections 2048;
}


http {
    include       mime.types;
    default_type  application/octet-stream;
    #日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for" "$request_body"';
    access_log /data/logs/$server_name.access.log main;
    
    root /data/html/hexo/public;
    index index.html index.php;

    #错误页面
    error_page  404 500 502 503 504 = /404.html;

    #关闭错误页面的nginx版本号
    server_tokens off;
    sendfile        on;
    #tcp_nopush      on;
    keepalive_timeout  65;
    client_max_body_size 10m;

    fastcgi_temp_file_write_size 128k;
    fastcgi_intercept_errors on;

    charset utf-8;    
    
    #开启gzip 
    gzip  on;
    gzip_min_length 1k;
    gzip_buffers 16 64k;
    gzip_http_version 1.1;
    gzip_comp_level 6;
    gzip_types text/plain application/x-javascript text/css application/xml application/json text/javascript application/x-httpd-php image/jpeg image/gif image/png;
    gzip_vary on;

    #https配置,全站同一个证书
    ssl_certificate /data/ssl/chained.pem;
    ssl_certificate_key /data/ssl/domain.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA;
    ssl_session_cache shared:SSL:50m;
    ssl_dhparam /data/ssl/dhparams.pem;
    ssl_prefer_server_ciphers on;
    resolver                   114.114.114.114 valid=300s;
    resolver_timeout           10s;
    
    #proxy设置
    proxy_connect_timeout  2s;
    proxy_read_timeout     2s;
    proxy_redirect off;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_temp_path   /home/www/temp;
    proxy_cache_path  /home/www/cache levels=1:2 keys_zone=cache_one:50m inactive=2h max_size=10g;
    
    #限流
    limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;
    
    include        conf.d/*.conf; 
    #防止恶意解析
    
    #autoindex on;
}
```

### 共有配置——common.conf

```Nginx
if ($http_user_agent ~ "DNSPod") {
    return 200;
}


#https证书申请使用
location /.well-known/acme-challenge/ {
    alias /data/challenges/;
    try_files $uri = 404;
}

#网站地图地址
location ~ /sitemap\.(html|xml)$ {
    expires off;
}

#防止图片盗链,30天的过期时间
location ~ .*\.(jpg|jpeg|gif|png|bmp|swf|fla|flv|mp3|ico|js|css)$ {
    valid_referers none blocked *.fanhaobai.com server_names ~\.google\. ~\.baidu\. ~\.len7.cc\.;
    if ($invalid_referer) {
	return  403;
    }
    access_log   off;
    expires      30d;
}

```

### 主站——www.conf

```Nginx
server {
    listen 443;
    server_name fanhaobai.com www.fanhaobai.com;

    #https配置
    ssl on;
    root  /data/html/hexo/public;
    
    if ($request_method !~ ^(GET|HEAD|POST)$ ) {
        return 444;
    }

    #fanhaobai.com重定向到www.fanhaobai.com
    if ($host ~ ^fanhaobai.com$) {
	return 301 https://www.fanhaobai.com$request_uri;
    }
   
    #微信二维码https代理 
    location /qrcode.php {
	proxy_set_header Host s.jiathis.com;
	proxy_pass	 http://s.jiathis.com$request_uri;
	expires max;
    }

    #豆瓣代理
    location ~ ^/douban/(.*)$ {
	proxy_set_header Host img3.doubanio.com;
	proxy_set_header Referer https://book.douban.com;
        proxy_pass       http://img3.doubanio.com/$1;
	expires max;
    }

    include conf.d/common;
}
 
server {
    #https认证使用
    listen 80;
    server_name fanhaobai.com www.fanhaobai.com;

    if ($request_method !~ ^(GET|HEAD|POST)$ ) {
        return        444;
    }

    include conf.d/common;	
    #重定向到https
    return    301  https://www.fanhaobai.com$request_uri;
}
```

### 维基——wiki.conf

```Nginx
server {
    listen 443;
    server_name wiki.fanhaobai.com;

    #https配置
    ssl on;
    
    root  /data/html/wiki;

    if ($request_method !~ ^(GET|HEAD|POST)$ ) {
        return 444;
    }

    try_files $uri $uri/ @rewrite;
    location @rewrite {
        if (!-e $request_filename) {
           rewrite  ^(.*)$  /index.php?s=$1  last;
           break;
        }
    }

    location ~ \.php {
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_split_path_info ^(.+\.php)(.*)$;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

    include conf.d/common;
}
 
server {
    #https认证使用
    listen 80;
    server_name wiki.fanhaobai.com;
        
    if ($request_method !~ ^(GET|HEAD|POST)$ ) {
        return        444;
    }

    include conf.d/common;
    return    301 https://$host$request_uri;
}
```

### 防止域名恶意解析

```Nginx
server {
    listen 80 default_server;
    server_name _;
    #引流到www.fanhaobai.com
    return 302  https://www.fanhaobai.com$request_uri;
}
```

## 限制恶意IP访问

为了保证站点安全可靠，本站开启了恶意 IP 的访问限制，[详情见这里](https://www.fanhaobai.com/2017/02/lock-ip.html)。

# 站点SEO

## Robots文件

```Bash
# robots.txt for www.fanhaobai.com 2017.03.01
User-agent: *
Disallow: /js
Disallow: /css
Disallow: /images
Sitemap: http://www.fanhaobai.com/sitemap.xml
```

## 站点地图

本站站点地图文件 [XML格式](http://www.fanhaobai.com/sitemap.xml)，并部署为自动更新。

**SEO专题文章 [»]()**

* [如何向搜索引擎提交链接](https://www.fanhaobai.com/2017/01/push-links.html)<span>（2017-01-17）</span>
* [自动更新站点地图的部署](https://www.fanhaobai.com/2017/01/update-sitemap.html)<span>（2017-01-16）</span>
* [Robots协议的那些事](https://www.fanhaobai.com/2017/01/robots.html)<span>（2017-01-12）</span>