# cookie 和 session 笔记

##　cookie

### 概述

原名 HTTP Cookie，通常直接叫做 cookie。

用于在客户端存储会话信息。

该标准要求服务器对任意 HTTP 请求发送 Set-Cookie HTTP 头作为响应的一部分，其中包含会话信息。

```python
# 响应头示例
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: asn=5; expires=Tue, 25-Dec-18 12:03:34 GMT; domain=www.douban.com; path=/

```

浏览器会存储 cookie 到本地，并在之后的同类请求中携带上 cookie 信息给服务器。

### 限制

1. cookie 可以限定域名和路径。
2. 每个域名可用的 cookie 数量是有限的，因浏览器而异。
3. cookie 大小也受浏览器限制。

### cookie 构成

* 名称。名称必须经过 URL 编码。
* 值。名称必须经过 URL 编码。
* 域（domain）。
* 路径（path）。
* 失效时间（expires）。格式为 GMT 格式的日期。如果不设置失效时间，则在浏览器关闭以后失效。
* 安全标志（secure）。secure 是 cookie 中唯一非键值对的部分。

### JavaScript 读取、写入、删除 cookie。

```javascript

var CookieUtil = {
    //注意： cookie 中的名字和值都是经过 URL 编码的需使用 decodeURIComponent 解码

    // 读取 cookie
    get: function (name) {
        
        // 查找 name
        var cookieName = encodeURIComponent(name) + "=",
            cookieStart = document.cookie.indexOf(cookieName),
            cookieValue = null;
        
        if (cookieStart > -1) {
            // 查找 name 之后的分号（分隔符）
            var cookieEnd = document.cookie.indexOf(";", cookieStart);
            // name 之后没有分号时
            if (cookieEnd == -1) {
                cookieEnd = document.cookie.length;
            }

            // 获取name=value这一段、解码
            cookieValue = decodeURIComponent(document.cookie.substring(cookieStart +
                cookieName.length, cookieEnd));
        }

        return cookieValue;
    },
    // 写入 cookie
    set: function (name, value, expires, path, domain, secure) {

        // 对 name 和 value 进行 URL 编码
        var cookieText = encodeURIComponent(name) + "=" +
            encodeURIComponent(value);
        
        // 转换日期格式，并追加
        if (expires instanceof Date) {
            cookieText += "; expires=" + expires.toGMTString();
        }

        // 路径
        if (path) {
            cookieText += "; path=" + path;
        }

        // 域
        if (domain) {
            cookieText += "; domain=" + domain;
        }

        // 安全
        if (secure) {
            cookieText += "; secure";
        }

        // 设置
        document.cookie = cookieText;
    },

    // 删除 cookie ,只需要设置失效时间为过去即可
    unset: function (name, path, domain, secure) {
        this.set(name, "", new Date(0), path, domain, secure);
    }
};

```


## session


