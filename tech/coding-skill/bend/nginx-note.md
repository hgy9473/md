# Nginx 学习笔记

> Nginx("engine x") 是一个高性能的 HTTP 和反向代理服务器，也是一个 IMAP / POP3 / SMTP 代理服务器。第一个版本发布于 2004 年 10 月。以稳定性、低系统资源消耗而闻名、并发能力强。

## Nginx 优点：

1. 高并发响应性能非常好。官方：处理静态文件请求可并发 5w/s。
2. 反向代理性能非常强。（可用于负载均衡）
3. 内存和 CPU 占用少。
4. 对后端服务有健康检查功能。

## Nginx 工作原理

* Nginx 由内核和模块组成。
  * 模块从结构上分为：核心模块、基础模块、第三方模块
  * 核心模块：HTTP模块、EVENT模块、MAIL模块
  * 基础模块：HTTP Access 、HTTP FastCGI、HTTP Proxy、HTTP Rewrite
  * 第三方模块：HTTP Upstream Request Hash、Notice、HTTP Access Key

* Nginx 的高并发得益于 epoll 模型。


## Nginx 安装 - CentOS

* 下载 Nginx 安装包，解压

```c
$ wget http://nginx.org/download/nginx-1.6.2.tar.gz
// 下载路径可到官网 http://ningx.org 获取

// 解压
$ tar -zxvf nginx-1.6.2.tar.gz
```

* 安装依赖项

```c
// 安装 Nginx 依赖
$ yum -y install gcc pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

* 配置安装参数

```c
// 进入解压目录
$ cd nginx-1.6.2/

// 配置 配置安装到/opt目录下
./configure --prefix=/opt/nginx --sbin-path=/usr/bin/nginx

```

* 编译并安装

```c
$ make && make install
```

* 启动nginx

```c
$ nginx

// 查看nginx是否已启动成功
$ ps -ef | grep nginx

// 停止nginx
$ nginx -s stop

$ pkill nginx

// 重新启动
$ nginx -s reload

// 测试 nginx 配置是否正确

$ nginx -t

```

## Nginx 常用命令

```c

// 查看 nginx 进程
$ ps -ef|grep nginx

// 平滑启动：意思是不停止 nginx 的情况下，重启nginx。重新加载配置文件，启动新的工作线程，完美停止旧的工作线程。
$ nginx -s reload

// 停止nginx
$ nginx -s stop

$ pkill nginx

// 测试 nginx 配置是否正确

$ nginx -t


// 平滑升级
// ...

```

## Nginx 配置文件详解

```python
# file: nginx.conf

user  nobody; 定义 Nginx 运行的用户，用户组

worker_processes  1; # 启动的进程数量

worker_cpu_affinity 00000001 # 为进程分配CPU

# 错误日志路径，日志级别有：debug | info | notice | warn | error | crit 
#error_log  logs/error.log; 
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

# 工作模式以及连接数上限
events {
    use epoll; # epoll 是多路复用 IO 的一种方式，可以大大提供nginx 性能。
    worker_connections  1024; # 单个进程最大并发连接数
    multi_accept on; # 尽可能多接受请求
}

# 设置 http 服务器
http {
    include       mime.types; # 设置mime类型
    default_type  application/octet-stream;

    # 设置日志格式
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    # 指定 nginx 是否调用 sendfile 来输出文件
    sendfile        on;

    #tcp_nopush     on; # 防止网络阻塞

    #autoindex on; #开启目录列表访问

    #keepalive_timeout  0;
    keepalive_timeout  65; # 连接超时时间（连接保持时间）

    #gzip  on; # 开启zip压缩

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html; # 服务器的默认网站根目录
            index  index.html index.htm; # 引导页
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # 配置 PHP 脚本请求全部转发到 FastCGI 处理
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```






## 参考资料
【1】 《Nginx高性能WEB服务器》. 网易云课堂
【2】 https://www.linuxidc.com/Linux/2017-02/140418.htm