# CentOS 安装配置 ArcGIS Notebook

## 1. 安装 Anaconda

下载 Anaconda3-2019.03-Linux-x86_64.sh 上传到 CentOS，然后执行

```c
// install
$ sudo bash Anaconda3-5.0.1-Linux-x86_64.sh
```

检查（增加）环境变量

```c
// check
$ vim /root/.bashrc

// 检查是否存在如下设置，没有则添加、保存
export PATH="/root/anaconda3/bin:$PATH"

// 检查文件后执行
$ source /root/.bashrc
```

检查是否安装完成：

（1）在终端中输入命令condal list，如果Anaconda被成功安装，则会显示已经安装的包名和版本号。

（2）在终端中输入python。这条命令将会启动Python交互界面，如果Anaconda被成功安装并且可以运行，则将会在Python版本号的右边显示“Anaconda”。

## 2. 配置 Jupyter

> Anaconda 包含了 Jupyter notebook  

> Jupyter配置文件需要手动生成，生成的文件中是被注释的默认配置，

生成配置文件

```c
// generate config file
jupyter notebook --generate-config
```

修改配置文件

```c
// modify 
$ vim /root/.jupyter/jupyter_notebook_config.py

// edit content is as follows:
c.NotebookApp.allow_remote_access = True

c.NotebookApp.ip = '*'

c.NotebookApp.open_browser = False

c.NotebookApp.port = 8888

```

设置登录密码

```c
// set password
jupyter notebook password
```

启动 notebook

```c
// start
$ (nohup jupyter notebook --allow-root --ip=0.0.0.0 > deep.log &)
```

启动后使用浏览器访问 <公网IP>:端口/tree，然后输入登录密码，即可使用。

## 3. 安装 arcgis package

在终端执行

```c
// install arcgis package
$ conda install -c esri arcgis
```

重启 Jupyter 即可使用 Python API for ArcGIS

测试代码

```python
# start python api for arcgis
from arcgis.gis import GIS
gis = GIS()
gis.map()
```

更多关于 ArcGIS Notebook 参考 [ArcGIS API for Python](https://developers.arcgis.com/python/guide/install-and-set-up/#Install-using-Anaconda-for-Python-Distribution)

（完）




