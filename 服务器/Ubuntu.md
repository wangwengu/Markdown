# 服务器相关操作

## 登录服务器

>`ssh ubuntu@ip地址`
>
>注意：腾讯云是`ubuntu`，阿里云是`root`

## 向服务器上传文件夹

> 上传文件夹 `scp -r 文件夹路径 服务器名:路径`
>
> `!!` 注意 `-r` 的位置

## 配置别名和免密登录

> 1. 创建 `~/.ssh/config` 文件
>
> 2. 写入如下内容
>
>    ```markdown
>    Host 别名
>    	HostName ip地址
>    	User ubuntu # 阿里云是root
>    ```
>
> 3. 配置免密登录
>
>    1. 生成密钥 `ssh-keygen`
>    2. 第二步添加公钥到服务器 `ssh-copy-id 别名`

# 用户相关操作

## 添加用户

>   1.   `adduser 用户名`

## 安装 `sudo` 权限

>   1.   `apt-get install sudo`

## 给用户添加 `sudo` 权限

>   1.   `usermod -aG sudo 用户名`

## 登录用户

>   1.   `ssh 用户名@ip地址 -p 端口号`

## 查看所有用户

>   1.   `cat /etc/shadow`

## 查看内存使用情况

>   1.   `free -h`

## 查看硬盘使用情况

>   1.   `df -h`

## 查看当前目录硬盘使用情况

>   1.   `du -sh`

## 删除软件

>   1.   `sudo apt-get remove --purge 名称`

## 下载图片

>   1.   `wget --output-document=图片别名 图片地址 图片存储地址`

## 查看所有运行的进程

>   1.   `top`
>   2.   ![541752](img/541752.png)

## 复制文件

>   1.   `cp A B` 将 `A` 复制到 `B` 下 

## 复制文件夹

>   1.   `cp -r A B` 将 `A` 文件夹复制到 `B` 文件夹下, 记得添加 `-r` 参数

## 

# 报错汇总

> 1. [ssh 报错 Broken pipe 解决方法](https://blog.csdn.net/whatday/article/details/113750935)
> 1. 不能输入中文
