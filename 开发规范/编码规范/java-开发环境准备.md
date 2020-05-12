# Java 开发环境准备

## 配置 OpenJDK 环境变量

[AdoptOpenJDK-8](https://adoptopenjdk.net/)

## 项目构建：Gradle
[Gradle-6.4](https://gradle.org/install/)

## 版本管理：git
- windows 安装步骤

官网下载终端安装即可 https://git-scm.com/download/win

- 配置 git
打开 git 终端，输入以下命令配置用户和邮箱
```
# 对你的 commit 操作设置关联的用户名
git config --global user.name "[name]"

# 对你的 commit 操作设置关联的邮箱地址
git config --global user.email "[email address]"
```

## Idea 工具

### 配置格式化模板
参考 [java-代码统一规范](java-代码统一规范.md)

### 配置头部注释
设置 -> File and Code Templates -> Includes ，对应 Header 配置为
```
/**
 * 描述：
 *
 * @author 『开发者名字』 by ${DATE}
 */
```
### 推荐插件
- Alibaba Java Coding Guidelines
- Lombok
- SonarLint

