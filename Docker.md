#### Docker教程

##### 下载安装 [地址](https://docs.docker.com/engine/install/ubuntu/)

> 1. 从 `Set up the repository` 开始一行行代码执行
> 2. `Install Docker Engine` 安装完结束即可

##### 镜像问题 [镜像地址](https://hub.docker.com/)

> 1. 拉取镜像 `docker pull 镜像名`
>2. 删除镜像 `docker image rm 镜像名`
> 2. 解压本地镜像`Tar`文件 `sudo docker load -i 镜像名`

##### 容器问题

> 1. 创建并运行容器 `docker run -p A:B -p C:D --name 别名 -itd 镜像名称`
>   + `-p A:B` 将B端口映射到A端口
>    + 多个端口映射需要填写多个`-p`即可
> 2. 启动容器 `docker start 容器名`
>
> 3. 停止容器 `docker stop 容器名`
>
> 4. 删除容器 `docker rm 容器ID`
>
> 5. 进入容器 `docker attach 容器名`
>
> 6. 重命名容器 `docker rename 旧容器名 新容器名`
>
> 7. 挂起容器 `先按 ctrl-p 再按 ctrl-q`
>
> 8. 将容器保存成镜像，将 `CONTAINER_NAME` 替换成容器名称
>
>    ```dockerfile
>    docker commit CONTAINER_NAME django_lesson:1.1
>    ```
>
> 9. 使用保存的镜像重新创建容器，将 `CONTAINER_NAME` 替换成容器名称
>
>    ```dockerfile
>    docker run -p 20000:22 -p 8000:8000 -p 80:80 -p 443:443 --name CONTAINER_NAME -itd django_lesson:1.1
>    ```

##### SSH 登录问题

> 1. 进入容器 `docker attach 容器名`
> 2. 执行命令 `apt-get update`
> 3. 执行命令 `apt-get upgrade`
> 4. 安装VIM  `apt-get install vim`
>    + 选择地区 `Asia`
>    + 选择地区 `Shanghai`
> 5. 安装SSH服务器 `apt-get install openssh-server`
> 6. 设置密码 `passwd`
> 7. 修改配置文件
>    + 进入配置文件 `vim /etc/ssh/sshd_config`
>    + 注释此行 `PermitRootLogin prohibit-password`
>    + 添加此行 `PermitRootLogin yes`
>    + 保存退出
> 8. 重启SSH配置文件 `/etc/init.d/ssh restart`
> 9. SSH登录即可 `ssh root@IP地址 -p 端口号`