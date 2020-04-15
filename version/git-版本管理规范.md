## 一、版本控制规范概述
无特殊说明，项目默认使用 [Git](https://git-scm.com/) 作为版本控制系统，内部平台使用 [GitLab](http://dev-git.gaolvzongheng.com/) 进行仓库建设，项目分支管理规范采用 [GitFlow](https://datasift.github.io/gitflow/IntroducingGitFlow.html) 模型（具体项目根据实际情况调整）。

## 二、版本号约定
遵循 [语义化版本 2.0.0](https://semver.org/lang/zh-CN/)

## 三、Git Flow 控制流程约定
<img src="https://nvie.com/img/git-model@2x.png" alt="git-flow" style="zoom:50%;" />

### 1.分支命名规范
分支命名
- master
- develop

分支前缀命名
- feature/
- release/
- hotfix/

```shell
# 分支命名示例
master
develop

feature/shop
feature/user

hotfix/v1.0.0

release/v1.0.0
release/v1.0.1
```
### 2. 分支详情
#### master
- 分支说明：线上稳定版本
- 注意事项：
  - 不可直接 push 到远程仓 master 分支
  - 只能被 release 分支或 hotfix 分支合并
#### develop
- 分支说明：当前最新开发成果的分支
- 注意事项：
  - 不可直接 push 到远程仓库 develop 分支
  - 不能与 master 分支直接交互
#### feature
- 分支说明：功能分支，每个功能需分别建立自己的子分支
- 注意事项：
  - 每个 feature 分支颗粒要尽量小，以利于快速迭代和避免冲突
  - 当其中一个 feature 分支完成后，它会合并回 develop 分支
  - 由每组开发管理员负责把所有 feature 分支开发完成的代码合并到 develop 分支
  - feature 分支只与 develop 分支交互，不能与 master 分支直接交互
#### release
- 分支说明：待发布分支，下个版本需上线的版本
- 注意事项：
  - release 分支一旦建立就将独立，不可再从其他分支 pull 代码
  - 必须合并回 develop 分支和 master 分支
  - release 分支测试通过后，合并到 master 分支并且给 master 标记一个版本号
#### hotfix
- 分支说明：紧急修复分支,用来快速给已发布产品修复 bug 或微调功能
- 注意事项：
  - 只能从 master 分支指定 tag 版本衍生出来
  - 一旦完成修复 bug，必须合并回 master 分支和 develop 分支
  - master 被合并后，应该被标记一个新的版本号
  - hotfix 分支一旦建立就将独立，不可再从其他分支 pull 代码

### 3. 工作中遇到的开发场景

#### 新需求
- 创建一个 feature/版本号-功能点的分支，如果 存在 未测试完毕的需求，就基于 master 创建，否则就基于 develop 创建
- 开发完成后将代码合并到 develop 供测试人员测试
- 测试人员在 develop 测试通过后，负责人再将代码发布到 release 供测试人员测试
- 测试人员在 release 测试通过后，负责人再将代码发布到 master 供测试人员测试
- 测试人员在 master 测试通过后，研发人员需要删除 feature/版本号-功能点这个分支

#### 修复测试环境 Bug
- 如果开发工时 < 2h，直接在 develop 开发，否则就要创建 feature 分支，流程与新需求加入的流程一致。

#### 修改预上线环境 Bug
- 在 release 测试出现了 Bug，首先要确认 develop 分支是否同样存在这个问题
- 如果存在，release 分支回滚到前一个版本，修复流程和修复测试环境 Bug 流程一致
- 如果不存在，这种可能性比较少，大部分是数据兼容问题，环境配置问题等
#### 修改正式环境 Bug
- 在 master 测试出现了 Bug，首先把 master 分支回滚到前一个版本保证线上没有问题，
- 确认 release 和 develop 分支是否同样存在这个问题，如果存在，修复流程 与 修复测试环境 Bug 流程一致。
- 如果不存在，这种可能性也比较少，大部分是数据兼容问题，环境配置问题等

#### 紧急修复正式环境 Bug
- 如果 release 分支存在未测试完毕的需求，就基于 master 创建 hotfix-xxx 分支，修复完毕后发布到 master 验证，验证完毕后，将 master 代码合并到 release 和 develop 分支，同时删掉 hotfix-xxx 分支
- 如果 release 分支不存在未测试完毕的需求，但 develop 分支存在未测试完毕的需求，就基于 release 创建 hotfix-xxx 分支，修复完毕后发布到 release 验证，后面流程与上线流程一致，验证完毕后，将 master 代码合并到 develop 分支，同时删掉 hotfix-xxx 分支
- 如果 release 和 develop 分支都不存在未测试完毕的需求，流程与修复测试环境 Bug 流程一致

### 4.分支管理常用命令（合并到常用命令中）
```shell
#origin 就是一个名字，它是在你 clone 一个托管在 Github 上代码库时，git 为你默认创建的指向这个远程代码库的标签.ps : 远程库也可以改叫其他名字 origin 是可以改的
#origin 指向的是 repository(知识库,远程仓库)，master 只是这个 repository 中默认创建的第一个 branch。当你 git push 的时候因为 origin 和 master 都是默认创建的，所以可以这样省略

#关联远程仓库
git remote add origin git@github…….git

#取消本地目录下关联的远程库, 重新关联其他仓库执行上面操作
git remote remove origin

#查看本地分支
git branch

#查看远程分支
git branch -r

#查看本地分支与远程分支
git branch -a

#创建本地分支
git branch branchname
git checkout -b branchname

#有时候删除远程分支会报 git delete remotes: remote refs do not exist 错误信息, 提示远程分支不存在
#原因是我们用 git fetch 保存到本地的缓存信息而已, 只需要先执行一下 git fetch -p origin 命令即可

#删除本地分支
git branch -d branchname

#删除本地暂存分支
git branch -dr origin/branchname

#删除远程分支
git push origin :branchname

#删除远程分支
git push origin --delete branchname

#创建一个分支并关联远程分支的完整流程
# 1. 创建本地分支
git branch branchname

#2. 切换到分支 branchname
git checkout branchname

# 1 和 2 可以合成一步 创建并切换到新分支
git checkout -b branchname

#3. 推送本地分支到远程库
git push origin branchname

#4.建立远程连接,然后就可以 pull 和 push 了
git branch --set-upstream-to origin/branchname

#3 和 4 可以合并成一步 推送本地分支到远程库,分支名一模一样,并同时建立连接
#这种方式更通用
git push -u origin branchname

#git push --set-upstream-to origin/branchname 和 git push -u origin branchname 的区别
- 如果我们本地 dev 分支需要关联远程库 , 第一种方式如果远程没有 dev 分支, 会报错关联不起
- 第二种方式 , 如果没有就会创建一个 dev 的远程分支然后再关联
- 推荐使用第二种,也是比较常用的
```

## 四、Git 提交信息规范

参考 [git-commit-统一规范.md](./git-commit-统一规范.md)

## 五、工具推荐

- [gitkraken Git 客户端 ](https://www.gitkraken.com/)
- [Sourcetree Git 客户端 ](https://www.sourcetreeapp.com/)
- [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

