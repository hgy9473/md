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

### cookie 用途

1. 通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。

2. cookie 使基于无状态的 HTTP 协议记录稳定的状态信息成为了可能。

3. 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）。

4. 个性化设置（如用户自定义设置、主题等）。

5. 浏览器行为跟踪（如跟踪分析用户行为等）



### 限制

1. cookie 可以限定域名和路径。

2. 每个域名可用的 cookie 数量是有限的，因浏览器而异。

3. cookie 大小也受浏览器限制。

### cookie 构成

* 名称。名称必须经过 URL 编码。

* 值。名称必须经过 URL 编码。

* 域（domain）。如果不指定，默认为当前文档的 host。

* 路径（path）。

* 失效时间（expires）。格式为 GMT 格式的日期。如果不设置失效时间，则在浏览器关闭以后失效。

* 安全标志（secure）。标记为 secure 的 cookie 只能通过被 HTTPS 协议加密过的请求发送给服务端。

* HttpOnly。为避免跨域脚本 (XSS) 攻击，通过JavaScript的 Document.cookie API无法访问带有 HttpOnly 标记的Cookie，它们只应该发送给服务端。如果包含服务端 Session 信息的 Cookie 不想被客户端 JavaScript 脚本调用，那么就应该为其设置 HttpOnly 标记。

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

### 概述

session 机制 是一种服务器端的机制，服务器使用一种类似于散列表的结构（也可能就是使用散列表）来保存信息。

当程序需要为某个客户端请求一个 session 的时候，服务器首先检查这个客户端的请求里是否已包含了一个 session 标识（称为session id）。如果已包含则说明以前已经为此客户端创建过session，服务器就按照session id把这个session检索出来使用。如果客户端请求不包含 session id，则为此客户端创建一个 session 并且生成一个与此 session 相关联的 session id。session id 的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串。这个session id 将被在本次响应中返回给客户端保存。保存这个 session id 的方式可以采用 cookie，这样在交互过程中浏览器可以自动的按照规则把这个标识发挥给服务器。

### cookie 被禁用时

由于 cookie 可以被人为的禁止，必须有其他机制以便在 cookie 被禁止时仍然能够把session id传递回服务器。

经常被使用的一种技术叫做 URL 重写，就是把session id直接附加在URL路径的后面。

另一种技术叫做表单隐藏字段。就是服务器会自动修改表单，添加一个隐藏字段，以便在表单提交时能够把session id传递回服务器。

### session 删除

除非程序通知服务器删除一个 session，否则服务器会一直保留，程序一般都是在用户做log off 的时候发个指令去删除 session。然而浏览器从来不会主动在关闭之前通知服务器它将要关闭，因此服务器根本不会有机会知道浏览器已经关闭。

大部分 session 机制都使用会话 cookie 来保存 session id，而关闭浏览器后这个session id 就消失了，再次连接服务器时也就无法找到原来的 session。

由于关闭浏览器不会导致 session 被删除，迫使服务器为 seesion 设置了一个失效时间，当距离客户端上一次使用 session 的时间超过这个失效时间时，服务器就可以认为客户端已经停止了活动，才会把 session 删除以节省存储空间。



## 参考资料

【1】《JavaScript 高级程序设计》第三版 P629

【2】《HTTP cookies》. MDN . [https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)

【3】《session》 . CSDN . https://blog.csdn.net/keda8997110/article/details/16922815

【4】《Java Web(三) 会话机制，Cookie和Session详解》 . CSDN . https://www.cnblogs.com/whgk/p/6422391.html

【5】《 HTTP无状态协议和 cookie、session 原理》 . segmentfault . https://segmentfault.com/a/1190000009518499



