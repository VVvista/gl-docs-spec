# Java 开发环境准备

## 配置 OpenJDK 环境变量
[AdoptOpenJDK-8](https://adoptopenjdk.net/)

## 项目构建：Gradle
[Gradle-6.4](https://gradle.org/install/)

## 版本管理：git
- windows 安装步骤

官网下载终端安装即可 https://git-scm.com/download/win

- 配置 git
打开 git 终端，输入以下命令配置用户和邮箱 (企业邮箱)
```
# 对你的 commit 操作设置关联的用户名
git config --global user.name "[name]"

# 对你的 commit 操作设置关联的邮箱地址
git config --global user.email "[email address]"
```

## Idea 工具

### 配置格式化模板
参考 [java-代码统一规范 ](java-开发规范.md)

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
官方插件地址-[JetBrains Plugins Repository](https://plugins.jetbrains.com/)

- [Lombok](https://plugins.jetbrains.com/plugin/6317-lombok)
- [代码检测：Alibaba Java Coding Guidelines](https://plugins.jetbrains.com/plugin/10046-alibaba-java-coding-guidelines)
- [代码检测：SonarLint](https://plugins.jetbrains.com/plugin/7973-sonarlint)
- [RestfulToolkit](https://plugins.jetbrains.com/plugin/10292-restfultoolkit)
- [类调用时序图：SequenceDiagram](https://plugins.jetbrains.com/plugin/8286-sequencediagram/)
- [Mybatis 插件集合：MyBatisCodeHelperPro](https://plugins.jetbrains.com/plugin/9837-mybatiscodehelperpro)
- [控制台日志 高亮：Grep Console](https://plugins.jetbrains.com/plugin/7125-grep-console/)
- [Kubernetes](https://plugins.jetbrains.com/plugin/10485-kubernetes)
- [Selenium UI Automation Testing](https://plugins.jetbrains.com/plugin/13691-selenium-ui-automation-testing)
- [Jenkins Control Plugin](https://plugins.jetbrains.com/plugin/6110-jenkins-control-plugin)
