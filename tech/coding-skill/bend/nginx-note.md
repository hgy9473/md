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

// 重新启动
nginx -s reload

// 测试 nginx

$ nginx -t

```




## 参考资料
【1】 《Nginx高性能WEB服务器》. 网易云课堂
【2】 https://www.linuxidc.com/Linux/2017-02/140418.htm