#### 教程

##### 初始化Django项目

> 1. 初始化Django`django-admin startproject 项目名`
> 2. 在`settings.py`将地区修改为`TIME_ZONE='Asia/Shanghai'` 重启项目
> 3. 在`settings.py`将`static`文件夹加入地址路径`STATIC_ROOT = os.path.join(BASE_DIR, 'static')`
> 4. 在`settings.py`将当前新建的`app`加入到Django项目中在`INSTALLED_APPS`中加入`game.apps.GameConfig`

##### 启动项目

> 1. 运行Django`python3 manage.py runserver 0.0.0.0:8000`

##### 创建app

> 1. 创建app`python3 manage.py startapp 名称`

##### 创建管理员用户

> 1. 创建管理员用户`python3 manage.py createsuperuser`
>    + !!邮箱可以不设置!!

##### 注意事项

> 1. 每一个python文件夹都有一个`__init__.py`文件
>    + !!不加0.0.0.0:8000 有些情况下会出错
