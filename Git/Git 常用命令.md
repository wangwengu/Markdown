# 配置

> 1. 配置全局用户名`git config --global user.name 用户名`
>    + `git config --global user.name wangwengu`
> 2. 配置全局邮箱地址`git config --global user.email 邮箱地址`
>    + `git config --global user.email 1339732832@qq.com`
> 3. 配置中文不乱码
>    + 命令行`git config --global core.quotepath false`
> 4. 生成公钥`ssh-keygen`
> 5. 将`.ssh` 下的公钥传到`github SSH`中

# Git暂存区

> 1. 撤销此次<font style="color: red">提交</font> `git rm -r --cached 文件名`
>
>    + `.` 代表所有文件
>    + 否则填写具体文件名即可
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
>          >
>        + 产生原因
>
>          > 文件内容发生改动，产生冲突
>          >
>        + 解决方案
>
>          > 先 `git add .` 更新文件 在重新操作即可
>
> 2. 撤销此次 `git add .` 的操作, 用如下命令重置
>
>    ```bash
>    git reset .
>    ```
>
> 3. 将所有.o文件移出暂存区 `git restore --stage *.o`
>
> 4. 将可执行文件main移出暂存区 `git restore --stage main`

# Git版本和拉取

> 1. 将历史版本在一行中进行显示，且索引值会全部显示`git log --pretty=oneline`
> 2. 将历史版本在一行中进行显示，且索引值显示前7位`git log --oneline`
> 2. 查看 `HEAD` 指针的移动历史（包括被回滚的版本）`git reflog --pretty=oneline`
> 3. 回滚某个特定的历史版本`git reset --hard 版本号（索引值的前7位）`
> 4. 拉取某个分支的某个版本
>    + 拉取整个项目 `git clone HTTPS地址`
>    + 切换分支 `git checkout branch_name`
>    + 获取版本信息【`1` 和 `2`】
>    + 回滚某个版本【`3`】

# Git分支

> 1. 查看本地所有分支`git branch`
> 2. 查看远程仓库所有分支`git branch --all`
> 3. 切换分支`git checkout branch_name`
> 4. 克隆所有分支
>    + `git clone 地址` 克隆代码
>    + `git branch --all` 查看所有分支
>    + `git checkout 分支名` 自动转到分支上，同时会将代码下载下来

# Git提交远程仓库

## SSH 提交 （不推荐）

### 原因

> 同一个账号创建多个仓库的时候，得创建多个 `ssh` 不同密钥，麻烦
>

### 提交步骤

> 1. 只能链接一个仓库，原因如上
> 1. 链接远程仓库`git remote add origin 仓库的SSH地址`
>    + 例如`git remote add origin git@github.com:wangwengu/Spiders.git`
>    + 错误`failed to push some refs to 'git@github.com:wangwengu/Spiders.git'`
>    + 原因：不同步造成的
>    + 解决方案：先`git pull origin master` 再`git push origin master`
> 2. 选择SSH提交，形如`git@github.com:wangwengu/Django.git`
>    + 血的教训！！卧槽了
> 3. 忽略某些文件夹或者文件 创建`.gitignore`文件
>    + 忽略文件夹`**/__pycache__` 忽略__pycache__文件夹
>    + 忽略文件`*.swp` 忽略所有后缀名为swp的文件
>      + <font style="color:red">**忽略多个文件时，一行写一个，不能在一行里面填写**</font>

## 使用HTTP提交（推荐）

### 提交步骤

> 1. 同`SSH` 提交步骤
> 2. 不同点：务必选择`HTTP` 提交，形如以`https://`

### 常见问题汇总

#### 问题 `1`

> + 问题描述
>
>   > **! [rejected]**    **master -> master (fetch first)**
>  >
>   > **error: failed to push some refs to 'https://github.com/wangwengu/Markdown.git'**
> 
> + 问题原因
>
>   > `github` 中的 `README.md` 文件不在本地代码目录中
>
> + 解决方案
>
>   > 使用 `git pull --rebase origin master` 进行代码合并即可解决

#### 问题 `2`

> + 问题描述
>
>   > `git push` 之后需要输入用户名 `Username for 'https://github.com'` 和密码 `Password for 'https://wangwengu@github.com'`
>
> + 问题原因
> 
>   > `personal token access` 过期了
>
> + 解决方案
>
>   > 重新生成 `personal token access` 即可，同时需要注意勾选第一个选项卡✅的所有内容

#### 问题 `3`

> + 问题描述
>
>   > 输入 `git push` 之后，出现错误， `fatal: The current branch master has no upstream branch.`
>
> +  问题原因
> 
>   > 本地仓库跟远程仓库关联不起来，对应不上，不知道推到哪里去
>
> + 解决方案
>
>   + 方案一
>
>     > 1. 按照提示，运行命令 `git push --set-upstream origin master` 即可
>
>   + 方案二
>
>     > 1. 先运行命令 `git remote set-url origin 仓库地址`
>    >    + 注意区分 `HTTPS` 地址和 `SSH` 地址
>     > 2. 再运行命令 `git push --set-upstream origin master` 即可

#### 问题 `4`

>   +   问题描述
>
>       >   输入git branch 之后，出现错误，`(no branch, rebasing master)`
>
>   +   问题原因
>
>       >   本地提交了 `commit` 但是未 `push` 成功并且远程仓库和本地的 `commit` 有冲突，`Git` 无法自动解决冲突时，会切换到一个匿名分支，然后使用 `git branch` 发现报错 `“no branch, rebasing master”`
>       >
>       >   ![3111326](https://gitee.com/peter95535/image-bed/raw/master/img/3111326.png)
>
>   +   解决方案一
>
>       >   当前匿名分支下
>       >
>       >   +   解决完冲突，然后使用命令`git rebase --continue`，可以将代码合并到之前操作时的分支，再执行`git push origin master`就可以将变更推到远程仓库了
>       >   +   不解决冲突，然后使用命令`git rebase --continue`，可以将代码合并到之前操作时的分支，再执行`git push origin master`就可以将变更推到远程仓库了，此时，远程仓库会显示两个版本
>
>   +   解决方案二
>
>       >   创新新分支【创建方法参考 `Git` 分支】，如上图所示
