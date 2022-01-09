#### 服务器相关操作

##### 登录服务器

>`ssh ubuntu@ip地址`
>
>注意：腾讯云是`ubuntu`，阿里云是`root`

##### 向服务器上传文件夹

> 上传文件夹 `scp -r 文件夹路径 服务器名:路径`
>
> !! 注意 -r 的位置

##### 配置别名和免密登录

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

#### 用户相关操作

> 1. 添加用户 `adduser 用户名`
> 2. 安装sudo权限 `apt-get install sudo`
> 3. 给用户添加sudo权限 `usermod -aG sudo 用户名`
> 4. 登录用户 `ssh 用户名@ip地址 -p 端口号`
> 5. 查看所有用户 `cat /etc/shadow`
> 6. 查看内存使用情况 `free -h`
> 7. 查看硬盘使用情况 `df -h`
> 7. 查看当前目录硬盘使用情况 `du -sh`
> 7. 删除软件 `sudo apt-get remove --purge 名称`
> 7. 下载图片 `wget --output-document=图片别名 图片地址 图片存储地址`

#### 报错汇总

> 1. [ssh 报错 Broken pipe 解决方法](https://blog.csdn.net/whatday/article/details/113750935)
