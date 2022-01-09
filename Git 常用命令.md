#### 配置

> 1. 配置全局用户名 `git config --global user.name 用户名`
>    + `git config --global user.name wangwengu`
> 2. 配置全局邮箱地址 `git config --global user.email 邮箱地址`
>    + `git config --global user.email 1339732832@qq.com`
> 3. 配置中文不乱码
>    + 命令行`git config --global core.quotepath false`
> 4. 生成公钥 `ssh-keygen`
> 5. 将 `.ssh` 下的公钥传到 `github SSH`中

#### Git暂存区

> 1. 撤销此次提交 `git rm -r --cached 文件名`
>    + `.` 代表所有文件
>    
>    + 否则填写具体文件名即可
>    
>    + 注意：
>    
>      + 报错 `1`
>    
>        + 错误描述
>    
>          > **error: the following files have staged content different from both the**
>          >
>          > **file and the HEAD:**
>          >
>          > 文件夹名
>          >
>          > **(use -f to force removal)**
>    
>        + 产生原因
>    
>          > 文件内容发生改动，产生冲突
>    
>        + 解决方案
>    
>          > 先 `git add .` 更新文件 在重新操作即可
>    
> 2. 将所有.o文件移出暂存区 `git restore --stage *.o`
>
> 3. 将可执行文件main移出暂存区 `git restore --stage main`

#### Git版本

> 1. 将历史版本在一行中进行显示，且索引值会全部显示 `git log --pretty=oneline`
> 2. 将历史版本在一行中进行显示，且索引值显示前7位 `git log --oneline`
> 3. 回滚某个特定的历史版本 `git reset --hard 版本号（索引值的前7位）`

#### Git分支

> 1. 查看本地所有分支 `git branch`
> 2. 查看远程仓库所有分支 `git branch --all`
> 3. 切换分支 `git checkout branch_name`
> 4. 克隆所有分支
>    + `git clone 地址` 克隆代码
>    + `git branch --all` 查看所有分支
>    + `git checkout 分支名` 自动转到分支上，同时会将代码下载下来

#### Git提交远程仓库

##### SSH 提交 （不推荐）

+ 原因

  >同一个账号创建多个仓库的时候，得创建多个 `ssh` 不同密钥，麻烦

+ 提交步骤（只能链接一个仓库，原因如上）

> 1. 链接远程仓库 `git remote add origin 仓库的SSH地址`
>    + 例如 `git remote add origin git@github.com:wangwengu/Spiders.git`
>    + 错误 `failed to push some refs to 'git@github.com:wangwengu/Spiders.git'`
>    + 原因：不同步造成的
>    + 解决方案：先 `git pull origin master` 再 `git push origin master`
> 2. 选择SSH提交，形如 `git@github.com:wangwengu/Django.git`
>    + 血的教训！！卧槽了
> 3. 忽略某些文件夹或者文件 创建 `.gitignore`文件
>    + 忽略文件夹 `**/__pycache__` 忽略__pycache__文件夹
>    + 忽略文件 `*.swp` 忽略所有后缀名为swp的文件
>      + <font style="color:red">**忽略多个文件时，一行写一个，不能在一行里面填写**</font> 

##### 使用HTTP提交（推荐）

+ 提交步骤

  > 1. 同 `SSH` 提交步骤
  > 2. 不同点：务必选择 `HTTP` 提交，形如以 `https://`