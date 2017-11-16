# 在 CentOS 中搭建 Python + uwsgi + nginx 服务器环境
> CentOS 版本： CentOS Linux release 7.4.1708 (Core)

安装 yum

安装 wget

安装 setuptools

安装 pip

## 安装 Django
> ### Django Hello World
1. 执行命令
```c
$ django-admin.py startproject demosite
$ cd demosite
$ python manage.py runserver 0.0.0.0:8002
```

2. 用浏览器打开 http://localhost:8002 或 http://127.0.0.1:8002

---
## 安装 python-devel
> 安装 uwsgi 之前需要安装 python-devel
---
## 安装 uwsgi
> ### uwsgi Hello World

1. 选择一个文件夹新建文件 test.py 并写入一个函数如：
```python
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return "Hello World"

```
2. 启动服务
```c
$ uwsgi --http :8001 --wsgi-file test.py
```

3. 用浏览器打开 http://localhost:8001 或 http://127.0.0.1:8001