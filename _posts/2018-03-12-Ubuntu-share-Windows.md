---
tags: IT Systems Media
title: Ubuntu shares to Windows10
excerpt: Samba的运用
---

Also published in https://soandcandy.us

CC 4.0 BY-SA

## Ubuntu下共享Windows10文件给电视盒子 ##

* toc
{:toc}

----

![Ubuntu](https://assets.ubuntu.com/v1/c037fd75-ubuntu-logo.png)



在给路由刷机搭建简易的家庭多媒体中心之教程里，介绍了利用在openwrt中以Samba共享路由文件夹的方法。同理，在Ubuntu下共享Windows的文件夹，使它能够被其他设备访问（主要是电视盒子），我们也同样使用Samba。



#### Samba ####

Samba是Linux和Unix与Windows进行互动操作的标准套件。

可以在其[官网](https://www.samba.org/samba/what_is_samba.html)详细了解一下。



#### Ubuntu下安装Samba ####

在终端安装：

```bash
sudo apt-get install samba
```

可能根据提示会要求安装依赖。



#### 创建Samba用户 ####

```bash
sudo touch /etc/samba/smbpasswd
sudo smbpasswd -a 你的账户名称
```



#### 共享文件 ####

##### 如果是新建共享文件夹 #####

1. 创建共享文件夹；

   ```bash
   mkdir ~/share #当前用户目录下创建一个叫share的共享文件夹
   chomd 777 ~/share #为该共享文件夹设置权限
   ```

2. 修改Samba配置；

   打开Samba的配置文件`vi /etc/samba/smb.conf`，在文件最后添加共享信息：

   ```bash
   [share]
   	path = 共享的路径
   	available = yes
   	browseable = yes
   	public = yes
   	writable = yes
   ```

   如果发现vi输入有问题，例如方向键变成ABCD，那是因为ubuntu默认安装的是vim-tiny，我们需要换成vim-full：

   ```bash
   sudo apt-get remove vim-common
   ```

   卸载vim-tiny，然后：

   ```bash
   sudo apt-get install vim
   ```

   安装vim-full。现在就可以正常使用vim了。

   ​

   ###### 如果提示Samba配置是只读文件 ######

   那么需要启用root用户。在Ubuntu中，root用户默认不启用，它的密码是随机的。所以我们首先要给root用户设置密码：

   ```bash
   sudo passwd -u root #解锁密码，将产生一个没有密码的账户
   sudo passwd root #设置root的密码
   su root #切换用户到root，如果出现root@开头的代表启用成功
   ```

   进入/usr/share/lightdm/lightdm.conf.d/目录下，编辑器打开50-unity-greeter.conf文件，添加以下信息以便登录方便：

   ```bash
   user-session=ubuntu
   greeter-show-manual-login=true
   all-guest=false
   ```

   重启生效，以root进入系统。如果提示错误，则`vi /root/.profile`修改文件，找到mesg n，修改为tty -s && mesg n保存退出重启系统生效。这样以后就方便一点了。

   ​

   ##### 如果是共享已有的Windows文件夹 #####

   如同本例。进入Windows系统所在卷，进入共享的目录，右击设置文件夹，`本地网络共享`，设置一下权限即可。

   ​

3. 重启Samba服务。

```bash
sudo /etc/init.d/samba restart
```

现在你就可以smb方式访问你的共享了。ENJOY！


![](https://soandcandy.us/wp-content/uploads/2018/03/end.jpg)
