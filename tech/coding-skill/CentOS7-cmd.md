# CentOS 命令行使用笔记
## 切换用户
> su 命令让你在不登出当前用户的情况下登录为另外一个用户。
```c
// 切换到 root 用户，不退出当前用户
$ su root

// 切换回去
$ su hgy

```

## 在 VMware 中的 NAT 网络配置
> 参考资料：http://www.linuxidc.com/Linux/2016-08/133998.htm

1. 在物理机中 `控制面板\网络和 Internet\网络连接` 打开 `VMare Virtual Ethernet Adapter for VMnet8` 属性窗口。（1） 勾选 `Vmware Bridge Protocal` 。（2）双击 `Internet 协议版本 4（TCP/IPv4）` 打开属性窗口设置自动获取 IP。
![CentOS NAT 网络配置](./images/VMnet8-attr.png) 

2. 在 VMware 中设置网络连接为 `NAT 模式`。
![ VMware NAT 网络设置](./images/VMnet8-nat.png)

3. 在 CentOS 终端中进入 `/etc/sysconfig/network-scripts`
```c
$ cd /etc/sysconfig/network-scripts
```

4. 在终端输入 `ls` 查找 `ifcfg-e* ` 文件并记住文件名。打开文件，修改使 `BOOTPROTO=dhcp` 、`ONBOOT=yes`。
> 在 CentOS 中使用 vi 修改文件的笔记见 [CentOS-vi.md](./CentOS-vi.md)

![ CentOS 网络设置](./images/CentOS-nat.png)

5. 在终端输入命令 ` service network restart ` 重启网络连接。


## 检查网络是否可用
```c
$ ping www.baidu.com
```

## 下载文件
```c
$ wget http://sw.bos.baidu.com/sw-search-sp/software/3756358c42c34/npp_7.5.1_Installer.exe
```

## 删除文件
```c
$ rm npp_7.5.1_Installer.exe
```

## 清空屏幕
```c
$ clear
```

## 查看操作系统版本
```
cat /etc/redhat-release
```

## 新建文件夹
```
$ mkdir newfolder
```

## 移动文件夹
```c
// 移动 test 到根目录
mv test /
```


## 解压文件
> 参考资料：tar 解压 http://www.cnblogs.com/chjw8016/p/5953168.html
> 参考资料：zip 解压 https://www.cnblogs.com/chinareny2k/archive/2010/01/05/1639468.html
```c
// 解压 .tar.gz 文件
tar -xzvf file.tar.gz 
// 解压 .zip 文件
unzip file.zip
```


## NAT 网络连接模式下 VMware 物理主机访问 CentOS 虚拟机上的网站
1. 关闭物理主机防火墙。
2. 关闭虚拟机防火墙。
3. 获取虚拟机 ip。
4. 在 WMware 中打开 `编辑 -> 虚拟网络编辑器`。打开 【NAT 设置窗口】。单击【添加】按钮。
![主机访问虚拟机网络设置](./images/VMnet8-webshare.png)
5. 输入主机端口、类型（选 TCP）、虚拟机 IP 地址（第 3 步获取到的）、虚拟机端口。
6. 测试。用物理主机浏览器打开 http://localhost:刚才设置的主机端口。

## yum
> 参考资料：http://www.runoob.com/linux/linux-yum.html


