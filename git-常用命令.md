> 专栏原创出处：[github-源笔记文件 ](https://github.com/GourdErwa/review-notes/tree/master/language/java-puzzle) ，欢迎 Star，转载请附上原文出处链接和本声明。

[toc]
## 使用前的配置

**检查你的配置**
```shell script
git config --list --show-origin
# 查看所有的配置以及它们所在的文件

git config --list
# 所有 Git 当时能找到的配置
```

**全局配置**
配置文件在`git config --list --show-origin` 命令中可查看
```shell script
git config --global user.name "[name]"
# 对你的 commit 操作设置关联的用户名

git config --global user.email "[email address]"
# 对你的 commit 操作设置关联的邮箱地址

git config --global color.ui auto
# 启用有帮助的彩色命令行输出。
```

**本地工程配置**
配置文件在当前工程目录/.git/config 文件中
```shell script
git config --local user.name xxx
git config --local user.email xxx@xxx.com
```

## 创建仓库
当着手于一个新的仓库时，你只需创建一次。要么在本地创建，然后推送到 GitHub；要么通过 clone 一个现有仓库。
```shell script
git init
# 将现有目录转换为一个 Git 仓库

git clone [url]
# Clone（下载）一个已存在于 GitHub 上的仓库，包括所有的文件、分支和提交 (commits)
```
## 分支
分支是使用 Git 工作的一个重要部分。你做的任何提交都会发生在当前“checked out”到的分支上。使用 git status 查看那是哪个分支。
```shell script
git branch [branch-name]
# 创建一个新分支
# 说明：如果报错「fatal: 不是一个有效的对象名：'master'。」说明初始化仓库后还没有任何提交记录。

git checkout [branch-name]
# 切换到指定分支并更新工作目录 (working directory)

git merge [branch]
# 将指定分支的历史合并到当前分支。这通常在拉取请求 (PR) 中完成，但也是一个重要的 Git 操作。

git branch -d [branch-name]
# 删除指定分支
```
## 进行更改
浏览并检查项目文件的发展。
```shell script
git log
# 列出当前分支的版本历史

git log --follow [file]
# 列出文件的版本历史，包括重命名

git diff [first-branch]...[second-branch]
# 展示两个分支之间的内容差异

git show [commit]
# 输出指定 commit 的元数据和内容变化

git add [file]
# 将文件进行快照处理用于版本控制

git commit -m"[descriptive message]"
# 将文件快照永久地记录在版本历史中
```
## 重做提交
Erase mistakes and craft replacement history
```shell script
git reset [commit]
# 撤销所有 [commit] 后的的提交，在本地保存更改

git reset --hard [commit]
# 放弃所有历史，改回指定提交
```
> 小心！更改历史可能带来不良后果。如果你需要更改 GitHub（远端）已有的提交，请谨慎操作。如果你需要帮助，可访问 github.community 或联系支持 (support)。


## 同步更改
将你本地仓库与 GitHub.com 上的远端仓库同步
```shell script
git fetch
# 下载远端跟踪分支的所有历史

git merge
# 将远端跟踪分支合并到当前本地分支

git push
# 将所有本地分支提交上传到 GitHub

git pull
# 使用来自 GitHub 的对应远端分支的所有新提交更新你当前的本地工作分支。git pull 是 git fetch 和 git merge 的结合。
```
## 多 SSH-Key 生成及代理配置
### 生成一个新的 SSH Key
```shell script
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# 执行命令生成 ssh-key，需要指定相应的邮箱

> Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [如果不更改文件名直接回车 ]
# 推荐不使用默认文件名 id_rsa ，按实际场景重新命名文件名称，比如：/Users/you/.ssh/id_rsa_github_my

> Enter passphrase (empty for no passphrase): [输入密码，回车 ]
> Enter same passphrase again: [再次输入密码，回车 ]

$ ll /Users/you/.ssh
# 可以看到刚刚生成的 ssh-key id_rsa_github_my 文件
# 复制对应 id_rsa_github_my.pub 内容添加至 WEB 端配置 SSH 内容地方即可。
```

### 将 SSH Key 添加到不同的代理
```shell script
$ vim /Users/you/.ssh/config

# 配置文件参数
# Host : Host 可以看作是一个你要识别的模式，对识别的模式，进行配置对应的的主机名和 ssh 文件（可以直接填写 ip 地址）
# HostName : 要登录主机的主机名（建议与 Host 一致）
# User : 登录名（如 gitlab 的 username）
# IdentityFile : 指明上面 User 对应的 identityFile 路径
# Port: 端口号（如果不是默认 22 号端口则需要指定）

Host github.com
HostName github.com
Preferredauthentications publickey
IdentityFile ~/.ssh/id_rsa_github_my
Port 22

Host dev-git.gaolvzongheng.com
HostName dev-git.gaolvzongheng.com
Preferredauthentications publickey
IdentityFile ~/.ssh/id_rsa_github_gaolvgo
Port 22

Host 119.3.177.8
HostName 119.3.177.8
Preferredauthentications publickey
IdentityFile ~/.ssh/id_rsa_github_gaolvgo
Port 22
```

### 测试配置是否成功
```shell script
$ .ssh ssh -T git@github.com
Enter passphrase for key '/Users/liwei/.ssh/id_rsa_github_my':
Hi GourdErwa! You've successfully authenticated, but GitHub does not provide shell access.
```

## 术语表

- HEAD：代表你当前的工作目录。使用 git checkout 可移动 HEAD 指针到不同的分支、标记 (tags) 或提交

- .gitignore 文件：
有时一些文件最好不要用 Git 跟踪。这通常在名为 .gitignore 的特殊文件中完成。你可以在 [github.com/github/gitignore](https://github.com/github/gitignore) 找到有用的 .gitignore 文件模板。


## 参考
- [Git Cheat Sheets](https://github.github.com/training-kit/)
- [github-Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
