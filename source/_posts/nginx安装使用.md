---
title: Nginx文档
categories: 教程
tags:
- ng
---
### M1 安装NG
- 首先MAC安装了brew
- brew install nginx 安装NG
- brew services start nginx 启动NG 访问 localhost:8080
- NG配置 /opt/homebrew/etc/nginx/nginx.conf
- 默认安装目录 /opt/homebrew/opt/nginx/

安装成功的提示：
```text
==> nginx
Docroot is: /opt/homebrew/var/www
The default port has been set in /opt/homebrew/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.
nginx will load all files in /opt/homebrew/etc/nginx/servers/.
To start nginx:
brew services start nginx
Or, if you don't want/need a background service you can just run:
/opt/homebrew/opt/nginx/bin/nginx -g 'daemon off;'
```

### 基本命令操作
- 启动 ./nginx
- 关闭 ./nginx -s stop
- 重启 ./nginx -s reload
- nginx -V（大写） 可查看日志文件目录 配置文件目录等
```text
configure arguments: --prefix=/opt/homebrew/Cellar/nginx/1.21.3 --sbin-path=/opt/homebrew/Cellar/nginx/1.21.3/bin/nginx 
--with-cc-opt='-I/opt/homebrew/opt/pcre/include -I/opt/homebrew/opt/openssl@1.1/include' 
--with-ld-opt='-L/opt/homebrew/opt/pcre/lib -L/opt/homebrew/opt/openssl@1.1/lib' 
--conf-path=/opt/homebrew/etc/nginx/nginx.conf --pid-path=/opt/homebrew/var/run/nginx.pid 
--lock-path=/opt/homebrew/var/run/nginx.lock --http-client-body-temp-path=/opt/homebrew/var/run/nginx/client_body_temp 
--http-proxy-temp-path=/opt/homebrew/var/run/nginx/proxy_temp --http-fastcgi-temp-path=/opt/homebrew/var/run/nginx/fastcgi_temp 
--http-uwsgi-temp-path=/opt/homebrew/var/run/nginx/uwsgi_temp --http-scgi-temp-path=/opt/homebrew/var/run/nginx/scgi_temp 
--http-log-path=/opt/homebrew/var/log/nginx/access.log --error-log-path=/opt/homebrew/var/log/nginx/error.log --with-compat 
--with-debug --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_degradation_module 
--with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module 
--with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module 
--with-http_sub_module --with-http_v2_module --with-ipv6 --with-mail --with-mail_ssl_module --with-pcre --with-pcre-jit --with-stream 
--with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module
```

