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

### config配置文件解读

#### work_process

配置工作进程数目，根据硬件调整，通常等于cpu的核数或2倍cpu的数量

#### event

工作模式和连接数

##### work_connections

配置每个work进程连接数的上限，一个work=1024

#### http

配置http服务器，利用他的反向代理功能实现负载均衡

##### server

配置虚拟主机，可以有多个serve

多个serve之间，listen字段和server_name必须有一个不一样

##### location

用location拦截各种请求的配置

默认匹配    一般是根路径（域名+端口=根路径） ，但是一般配置项目会再复制一个，不用根路径
当访问的路径中有斜杆，会被该location匹配到进行处理

```js
location /{ 
    root  配置服务器的默认网站的根目录位置，默认为nginx安装目录下的html目录(本地磁盘的路径)
    index 配置首页文件的名称(index.html)
}
```

精准匹配

```
location=/person.html{
root 本地磁盘的路径
}
```

正则表达式的location

### 部署静态web服务器

#### 修改config文件

复制一份http里面的location

#### 配置路径用于访问

把资源文件放到nginx指定根路径（/opt/www）里面

这里需要放在/opt/www/ace文件夹里面，因为root里面的本地磁盘根路径，对应的是请求的根路径（/）。

就是说 当你在浏览器请求访问/ace页面的时候，实际上是访问 本地磁盘路径/opt/www里面的ace路径下的文件资源

```js
location /ace{ 
    root  /opt/www;
    index login.html
}
```

```js
192.168.115.128/ace===192.168.115.128/ace/login.html
```

### 负载均衡

#### 实现

通过在config文件配置实现负载均衡

```js
upstream www.webapp.com{
server 192.168.0.1:80880
server 192.168.0.1:80881
}

server:{
	location /webapp{ 
   	proxy_pass:www.webapp.com
	}
}
```

#### 策略

##### 轮询策略

比如有两台服务器，浏览器请求了两次，就会一台服务器接收到一次

##### 权重策略

每个请求按一定的比例分配到两台服务器

```js
upstream www.webapp.com{
server 192.168.0.1:80880 weight=5
server 192.168.0.1:80881 weight=2
}
```

##### 最少连接数策略

web请求会被转发到连接数最少的服务器上

```js
upstream www.webapp.com{
	least_conn;
server 192.168.0.1:80880 weight=5
server 192.168.0.1:80881 weight=2
}
```

##### ip hash策略

上面的三种策略都会涉及到转发到不同服务器，导致section丢失的问题

ip hash也叫IP绑定。每个请求按访问ip的hash值分配，这样每个访问客户端会固定一个后端服务器，可以解决section丢失的问题

```js
upstream www.webapp.com{
	ip_hash;
server 192.168.0.1:80880 weight=5
server 192.168.0.1:80881 weight=2
}
```

### 静态代理

把所有的静态资源都放在nginx上访问，而不是直接访问后台服务器，这种方式叫静态代理。

#### 两种方式

在config文件的location中配置一个静态资源的后缀实现

在config文件的location中配置静态资源所在的目录实现

```js
location ~.*(css|js|img|image){
root /opt/static;
}
```

### 动静分离



### nginx部署vue项目

前端build项目

把项目放到nginx指定目录下，然后配置nginx.config文件

路由模式下刷新显示404页面，在location里面加try_files配置字段