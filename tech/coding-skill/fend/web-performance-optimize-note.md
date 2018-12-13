# 网页加载性能优化方法

## 1. HTTP Gzip 压缩传输

> 问题：网络传输大文件（MB级别）的文件传输确实消耗很多时间，即便服务器搭建在本地也慢。
> 观察淘宝、天猫、京东以及各免费 CDN 网站，可以发现他们都用了 gzip 压缩。

![no gzip](../../images/cdntest_Cesium2.js.jpg)

### （1）Gzip 压缩简介

![illustrate gzip](../../images/illustrate-gzip.jpg)

* HTTP 协议支持 GZIP 压缩机制，也称协议压缩。 HTTP GZIP压缩是由 Web 服务器和浏览器共同遵守的协议，也就是说WEB服务器和浏览器都必须遵守。目前主流的服务器和浏览器都支持GZIP压缩技术。包括 IE、Chrome、FireFox、Opera 等；服务器有 Nginx, Tomcat、Apache 和 IIS 等。

* GZIP 主要用来压缩 html,css,javascript,等静态文本文件，也支持对动态生成的，包括CGI、PHP , JSP , ASP , Servlet,SHTML等输出的网页也能进行压缩。

* 压缩过程：客户端发送 HTTP 请求，如果请求头(Request Headers)中携带 Accept-Encoding: gzip,deflate (现在的浏览器一般默认都是这样)，那么 Web 服务器在传输响应内容之前，会对响应内容进行压缩，并在响应头中添加Content-Encoding:gzip。

* 解压过程：（浏览器）客户端接收到响应，如果响应头中包含 Content-Encoding:gzip，那么浏览器会自动将响应内容进行 GZIP 解压缩，然后再呈现在页面上。

### （2）Tomcat 配置 Gzip 压缩

修改 tomcat/conf/server.xml，并重启
```xml

<!-- 关键代码：compress*  -->  
<!-- 关键代码：useSendfile="false" 必须要有，否则不生效-->  
<Connector port="8090" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8453"
               executor="tomcatThreadPool"
               enableLookups="false" 
               URIEncoding="utf-8"
			   
               compression="on"
			   compressionMinSize="2048"
			   compressableMimeType="text/html,text/xml,text/plain,text/javascript,application/javascript,application/xml,application/json,application/rjson"
			   useSendfile="false"
			   
               />

<!--  注解：
    compression="on"  // 打开压缩功能 (on|off)
    compressionMinSize="2048" // 启用压缩的输出内容大小，这里面默认为2KB，即文件大于 2KB 才压缩
    compressableMimeType="text/html,text/xml,text/plain,text/css,application/javascript" //对哪些文件类型启用压缩 
-->

```

### （3）Nginx 配置 Gzip 压缩

参考[你真的了解 gzip 吗？](https://zhuanlan.zhihu.com/p/24764131)

### （4） 效果示例

> 测试说明：Tomcat 8 ，部署在本地

* 不使用 Gzip
![no gzip](../../images/cdntest_Cesium1.js.jpg)
![no gzip](../../images/cdntest_Cesium2.js.jpg)
![no gzip](../../images/cdntest_Cesium3.js.jpg)

不使用 Gzip 时传输的 Cesium.js 内容大小为 2.4M ,时间 1.50 秒

* 使用 Gzip
![with gzip](../../images/cdntest_Cesium-gzip.js.jpg)
![with gzip](../../images/cdntest_Cesium-gzip2.js.jpg)
![with gzip](../../images/cdntest_Cesium-gzip3.js.jpg)

使用 Gzip 时传输的 Cesium.js 内容大小为 911K ,时间 806 毫秒

### （6） 补充说明

* Gzip 压缩需要消耗额外的服务器资源。
* 对于已经压缩过的文件格式如：*.jpg, *.mp3, *.mp4, *.s3m 等，gzip 压缩效果不大，不建议启用压缩。




## 参考资料
【1】[Tomcat启用GZIP压缩，提升web性能](http://www.cnblogs.com/DDgougou/p/8675504.html)
【2】[Tomcat8使用gzip压缩JS,CSS文件](https://www.cnblogs.com/imaxue/p/6867324.html)