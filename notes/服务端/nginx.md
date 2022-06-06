### 定义

一个高性能的（静态）web服务器和反向代理服务器，也可以作为邮件代理服务器

### 特点

占有内存小，并发处理能力强，以高性能、低系统资源消耗而闻名

### 主要应用

静态网站部署  负载均衡  静态代理   动静分离   虚拟主机

### 反向代理和正向代理

反向代理是指以代理服务器来接收internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上的请求连接的客户端，此时代理服务器对外就表现为一个代理服务器。

nginx不处理用户请求，只是接收请求将请求转发给后台服务器，由后台服务器处理请求，利用nginx反向代理实现负载均衡

正向代理类似一个跳板机，代理访问内部资源。比如我是一个用户，我访问不了某个网站，但是能访问一个代理服务器，这个代理服务器，他能访问我不能访问的网站，于是我先连上代理服务器，告诉他我需要访问哪个网站，然后他再取回来返回给我（vpn）

### nginx.conf配置文件

vim /usr/local/nginx/conf/nginx.conf

##### 全局块

配置服务器整体运行的配置指令

##### event块

影响 Nginx 服务器与用户的网络连接

##### http 块

这算是 Nginx 服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里。

需要注意的是：http 块也可以包括 http 全局块、server 块

> http 全局块

http 全局块配置的指令包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单链接请求数上限等

> server 块

这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。
每个 http 块可以包括多个 server 块，而每个 server 块就相当于一个虚拟主机。
而每个 server 块也分为全局 server 块，以及可以同时包含多个 locaton 块

1. 全局 server 块,最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或 IP 配置
2. location 块,一个 server 块可以配置多个 location 块。这块的主要作用是基于 Nginx 服务器接收到的请求字符串（例如 server_name/uri-string），对虚拟主机名称（也可以是 IP 别名）之外的字符串（例如 前面的 /uri-string）进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行

```bash
# =========================全局块============================
# user user [grop];        # 指定可以运行nginx 服务器的用户及组。只被设置的用户及组才有权限管理
# user nobody nobody;      # 所用用户可用，默认配置
user nginx nginx; 
# worker_processes number | auto;   # 控制并发处理，值越大，支持的并发数越多
# worker_processes 1;               # 默认值 ，最多产生1个worker_processes
worker_processes auto;
# pid file;                  # pid 存放路径.相对或绝对路径
# pid logs/nginx.pid;        # 默认在安装目录logs/nginx.pid
pid /var/run/nginx.pid;
# error_log file | stder [debug | info | notice | warn | error | crit | alert | emerg];  #错误日志存放路径
# error_log logs/error.log error;  # 可在 全局块，http 块 ,serveer 块，location 块中配置
error_log /data/wwwlogs/error_nginx.log crit;
# include file;      # 配置文件引入，可以是相对/绝对路径。指令可以放在配置文件的任意地方(任意块中)
worker_rlimit_nofile 51200;
# ===========================events 块===============================
events {
#accept_mutex on|off;      # 网络连接序列化。解决“惊群”问题
accept_mutex on;           # 默认为开启状态
#multi_accept on|off;      # 是否允许worker process同时接收多个网络连接，
multi_accept off;          # 默认为关闭
#use method;               # 事件驱动模型选择，select 、 poll 、 kqueue 、 epoll 、 rtsig 、 /dev/poll 、 eventport
use epoll;
#worker_connections number; # worker process 的最大连接数。 
worker_connections 512;     # 默认值为512。注意设置不能大于操作系统支持打开的最大文件句柄数量。
}
# ==========================http块============================
http {
# 定义 mime-type (网络资源的媒体类型)。  指定能识别前端请求的资源类型 
include mime.types;   # 引入外部文件 ，在外部文件中有定义types 块
default_type application/octet-stream;   # 默认为 text/plain 。 http块、server块、location块中可配置
# access_log path [format[buffer=size]];  # 定义服务日志，记录前端的请求日志
# access_log logs/access.log combined;    # combined 是默认定义的日志格式字符串名称
access_log off;                          # 关闭日志
# log_format name stirng      # 定义了日志格式，以便access_log 使用
# log_format  combined  '$remote_addr - $remote_user [$time_local] "$request" '
#                  '$status $body_bytes_sent "$http_referer" '
#                  '"$http_user_agent" "$http_x_forwarded_for"';
# sendfile on|off;       # 配置允许 sendfile 方式传输文件
sendfile off;           # 默认关闭，可在http块、server块、location块 中定义
# sendfile_max_chunk size;    # 配置worker process 每次调用sendfile()传输的数据最最大值
# sendfile_max_chunk 0;        # 默认为0 不限制。  可在http块、server块、location块 中定义
sendfile_max_chunk 128k;
# keepalive_timeout timeout [header_timeout];    # 配置连接超时时间, 可在http块、server块、location块 中定义
# keepalive_timeout 75s;        # 默认为75秒，与用户建立会话连接后，nginx 服务器可以保持这些连接打开的时间。
keepalive_timeout 120 100s;    # 在服务器端保持连接的时间设置为120s,发给用户端的应答报文头部中keep-alive 域的设置为100s
# keepalive_requests number;    # 单连接请求数上限
keepalive_requests 100;        # 默认为 100
server_names_hash_bucket_size 128;
client_header_buffer_size 32k;
large_client_header_buffers 4 32k;
client_max_body_size 1024m;
tcp_nopush on;
server_tokens off;
tcp_nodelay on;
fastcgi_connect_timeout 300;
fastcgi_send_timeout 300;
fastcgi_read_timeout 300;
fastcgi_buffer_size 64k;
fastcgi_buffers 4 64k;
fastcgi_busy_buffers_size 128k;
fastcgi_temp_file_write_size 128k;
#Gzip Compression
gzip on;
gzip_buffers 16 8k;
gzip_comp_level 6;
gzip_http_version 1.1;
gzip_min_length 256;
gzip_proxied any;
gzip_vary on;
gzip_types
text/xml application/xml application/atom+xml application/rss+xml application/xhtml+xml image/svg+xml
text/javascript application/javascript application/x-javascript
text/x-json application/json application/x-web-app-manifest+json
text/css text/plain text/x-component
font/opentype application/x-font-ttf application/vnd.ms-fontobject
image/x-icon;
gzip_disable "MSIE [1-6]\.(?!.*SV1)";
# If you have a lot of static files to serve through Nginx then caching of the files' metadata (not the actual files' contents) can save some latency.
open_file_cache max=1000 inactive=20s;
open_file_cache_valid 30s;
open_file_cache_min_uses 2;
open_file_cache_errors on;
######################## default ############################
#    server {
#    listen 8067;
#    server_name _;
#    access_log /data/wwwlogs/access_nginx.log combined;
#    root /data/wwwroot/default;
#    index index.html index.htm index.jsp;
#    location /nginx_status {
#        stub_status on;
#        access_log off;
#        allow 127.0.0.1;
#        deny all;
#        }
#    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|ico)$ {
#        expires 30d;
#        access_log off;
#        }
#    location ~ .*\.(js|css)?$ {
#        expires 7d;
#        access_log off;
#        }
#    location ~ {
#        proxy_pass http://127.0.0.1:8080;
#        include proxy.conf;
#        }
#    }
#    server{
#     listen 8087;
#    server_name _;
#     root /home/wwwroot/default;
#     location / {
#      }
#    }
upstream backend{
server 127.0.0.1:8080 weight=1 max_fails=2 fail_timeout=2;
server 139.224.209.104:8080 backup;
}
server{
listen 80;
listen 443 ssl;
server_name wmcenter.xy-asia.com;
#ssl on;
default_type 'text/html';
charset utf-8;
ssl_certificate   /usr/local/cert/1634560_wmcenter.xy-asia.com.pem;
ssl_certificate_key  /usr/local/cert/1634560_wmcenter.xy-asia.com.key;
ssl_session_timeout 5m;
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
root html;
index index.html index.htm;
location ~ .*\.(js|css)?$
{
expires 1h;
proxy_pass   http://backend;
}
location / {
proxy_pass  http://backend;
add_header backendIP $upstream_addr;
# proxy_next_upstream error timeout invalid_header http_500 http_502 http_503;
# proxy_set_header Host $host;
# proxy_set_header X-Real-IP $remote_addr;
# proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
# proxy_set_header X-Forwarded-Proto https;
# proxy_redirect http://http://127.0.0.1:8080 $scheme://127.0.0.1:8080;
# port_in_redirect on;
# proxy_redirect     off;
include proxy.conf;
}
# error_log /home/wwwlogs/xywm.log;
}
# 缓存配置
proxy_temp_file_write_size 264k;
proxy_temp_path /data/wwwlogs/nginx_temp;
proxy_cache_path /data/wwwlogs/nginx_cache levels=1:2 keys_zone=prc:200m inactive=5d max_size=400m;
proxy_ignore_headers X-Accel-Expires Expires Cache-Control Set-Cookie; 
########################## vhost #############################
include vhost/*.conf;
}
```



```json
user nginx;A0
worker_processes 5; error_log /var/log/nginx/error.log warn; 
pid /var/run/nginx.pid; 
events{
    worker_connections 1024; 
    use epol1;
}
#负载均衡
http{
    upstream www.webapp.com{
    server 192.168.0.1:80880 weight=3
    server 192.168.0.1:80881 weight=1
}
server{
    listen 80
    server_name localhost
    location / {
    root html
    index index.html index.htm
    #反向代理
    proxy_pass http://kuangshenstudy;
		}
	}
}
```








### nginx部署vue项目

前端build项目

把项目放到nginx指定目录下，然后配置nginx.config文件

路由模式下刷新显示404页面，在location里面加try_files配置字段