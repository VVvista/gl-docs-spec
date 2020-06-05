# Jenkins 发布流程

## Jenkins 目录规范

Jenkins 新建任务选择文件夹，创建对应文件夹对应 gitlab organization, 对应关系如下:

```bash
android  文件夹 --> gitlab android  组
backend  文件夹 --> gitlab backend  组
frontend 文件夹 --> gitlab frontend 组
ios      文件夹 --> gitlab ios      组
ops      文件夹 --> gitlab ops      组
```

## Jenkins Jobs 规范

Jenkins jobs 在 Jenkins 所对应的目录下，对应 gitlab 的 project, 对应关系如下:

```bash
# 目录/Job，Job 名称和 gitlab 的 project 对应
jenkins ops/ansible  --> gitlab glzh/ops/ansible
```

## Jenkins 创建流水线规范

Jenkins 流水线类型，选择 `多分支流水线` , 触发通过 Jenkinsfile, 添加完成之后记得在分支源里 Default 字段，选择不通过 SCM 自动触发。


## Dockerfile

### Java 示例

在项目的主路径下创建名称为 Dockerfle 的文件,内容如下:

```bash
# 指定 jdk 镜像
FROM openjdk:11

# 运行的工作目录
WORKDIR /app

# 将 jar 包考入 docker 镜像中
COPY *.jar app.jar
```

## Jenkinsfile

### Java 示例

在项目的主路径下创建名称为 Jenkinsfile 的文件,内容如下

```groovy
// 引入 jenkins 共享库
@Library('devops') _

// 指定 maven pom.xml 路径, 默认 pom.xml
env.POM = 'pom.xml'

// 指定 java 版本
env.JAVA_VERSION = 11

// 指定模块相对路径,默认为代码父目录
env.MODULE_PATH = 'gl-lost-bootstrap'

// 指定 dockerfile 名称
env.DOCKER_FILE = 'Dockerfile'

// 指定构建镜像名称, 例如 docker.io/library/name:v1 中的 name 字段
env.IMAGE_NAME = 'gl-lost-bootstrap'

// 指定镜像版本
env.IMAGE_TAG = '4.0'

// 指定构建方法
buildJava()
```

## 触发 CI 构建流程

目前所有的分支都可以纳入 Jenkins Job 中，触发 Push 镜像的流水线为 Develop 分支，目前可以使用手动点击触发构建


## 权限管理

目前手动创建账号和分配项目构建权限

##  CD 持续部署

暂未实现
