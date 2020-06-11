
## 一、开发理念

- **用心** 保持责任心和敬畏心，以工匠精神持续雕琢。
- **可读** 代码无歧义，通过阅读而非调试手段浮现代码意图。
- **整洁** 认同《重构》和《代码整洁之道》的理念，追求整洁优雅代码。
- **一致** 代码风格、命名以及使用方式保持完全一致。
- **精简** 极简代码，以最少的代码表达最正确的意思。高度复用，无重复代码和配置。及时删除无用代码。
- **抽象** 层次划分清晰，概念提炼合理。保持方法、类、包以及模块处于同一抽象层级。
- **极致** 拒绝随意，保证任何一行代码、任何一个字母、任何一个空格都有其存在价值。

>--- 产出的代码是自己雕琢过的艺术品。

## 二、代码提交行为规范（强制性）

- 确保通过全部测试用例，确保执行`./mvnw clean install`可以编译和测试通过。
- ~~确保覆盖率不低于 master 分支。~~
- ~~确保使用 Checkstyle 检查代码，违反验证规则的需要有特殊理由 (待确认规范文件)~~。
- 应尽量将设计精细化拆分；做到小幅度修改，多次数提交，但应保证提交的完整性。
- 确保遵守编码规范。

### 1. 代码检测
Q: 为什么设定了多种检测工具？
A: 每种类型工具侧重点不同，我们使用多种工具最终的目的就是产出高质量优质代码，从基本的坏味道代码到性能问题逐步优化。

Q: 检测的标准是什么？
A: 除 Spotbugs 作为编译期强制进行检测外（目前不强制编译期检测），其余作为辅助检测，对于个人能处理的检测结果尽量处理，对于部分比较苛刻的检测规则可忽略。

Q: 忽略的规则如何统一？
A: 通过 GitLab 申请忽略检测 [gl-docs-spec/issues](https://dev-git.gaolvzongheng.com/glzh-docs/gl-docs-spec/issues) ,申请标题 [忽略检测-java-{BUG-CODE}]。

**1.1 Idea-Analyze**

> Idea 自带，阿里编码规约默认集成到该检测中 「Ali-Check」

- 代码检测 Analyze -> Inspect Code
- 矩阵依赖分析 Analyze -> Dependency Matrix 良好的代码依赖应该为「倒三角模式」

**1.2 阿里编码规约**
 
安装插件 [Alibaba Java Coding Guidelines](https://plugins.jetbrains.com/plugin/10046-alibaba-java-coding-guidelines) ，
开启实时扫描 `Tools -> 阿里编码规约 -> 打开实时扫描`，手动分析右键分析内容->SonarLint。

**1.3 SonarLint**

安装插件 [SonarLint](https://plugins.jetbrains.com/plugin/7973-sonarlint) ，
开启自动分析 `Preferences -> Tools -> SonarLint -> 勾选 Automatically trigger analysis`，手动分析右键分析内容-> 编码规约扫描。

**1.4 Spotbugs**

查看 [spotbugs-exclude.xml](.spotbugs-exclude.xml) 文档注释介绍。

### 2. 格式化
格式化配置参考『java-开发环境准备』内容，保证代码样式一致性。

## 三、编码规范
编码规范遵从于《阿里巴巴 JAVA 开发规约》，参考 [Alibaba-Java-Coding-Guidelines](https://alibaba.github.io/Alibaba-Java-Coding-Guidelines)

- 使用 Linux 换行符。
- 缩进（包含空行）和上一行保持一致。
- 类声明后与下面的变量或方法之间需要空一行。
- 不应有无意义的空行。
- 类、方法和变量的命名要做到顾名思义，避免使用缩写。
- 返回值变量使用`result`命名；循环中使用`each`命名循环变量；map 中使用`entry`代替`each`。
- 捕获的异常名称命名为`ex`；捕获异常且不做任何事情，异常名称命名为`ignored`。
- 配置文件使用驼峰命名，文件名首字母小写。
- 需要注释解释的代码尽量提成小方法，用方法名称解释。
- 注释请不用使用大白话进行描述。
- `equals`和`==`条件表达式中，常量在左，变量在右；大于小于等条件表达式中，变量在左，常量在右。
- 除了构造器入参与全局变量名称相同的赋值语句外，避免使用`this`修饰符。
- 除了用于继承的抽象类之外，尽量将类设计为`final`。
- 嵌套循环尽量提成方法。
- 成员变量定义顺序以及参数传递顺序在各个类和方法中保持一致。
- 类和方法的访问权限控制为最小。
- 方法所用到的私有方法应紧跟该方法，如果有多个私有方法，书写私有方法应与私有方法在原方法的出现顺序相同。
- 方法入参和返回值不允许为`null`。如果必须为 `null`，请使用 `@Nullable` 注解标识或者使用 `Optional<T>`。
- 优先使用三目运算符代替 if else 的返回和赋值语句。
- 优先使用 [lombok](https://projectlombok.org) 代替构造器，getter, setter 方法和 log 变量。
- 优先考虑使用`LinkedList`，只有在需要通过下标获取集合中元素值时再使用`ArrayList`。
- `ArrayList`，`HashMap`等可能产生扩容的集合类型必须指定集合初始大小，避免扩容。
- 注释只能包含 javadoc，todo 和 fixme。
- 公开的类和方法必须有 javadoc，其他类和方法以及覆盖自父类的方法无需 javadoc。

## 四、单元测试规范

- 测试代码和生产代码需遵守相同代码规范。
- 单元测试需遵循 AIR（Automatic, Independent, Repeatable）设计理念。
  - 自动化（Automatic）：单元测试应全自动执行，而非交互式。禁止人工检查输出结果，不允许使用`System.out`，`log`等，必须使用断言进行验证。
  - 独立性（Independent）：禁止单元测试用例间的互相调用，禁止依赖执行的先后次序。每个单元测试均可独立运行。
  - 可重复执行（Repeatable）：单元测试不能受到外界环境的影响，可以重复执行。
- 单元测试需遵循 BCDE（Border, Correct, Design, Error）设计原则。
  - 边界值测试（Border）：通过循环边界、特殊数值、数据顺序等边界的输入，得到预期结果。
  - 正确性测试（Correct）：通过正确的输入，得到预期结果。
  - 合理性设计（Design）：与生产代码设计相结合，设计高质量的单元测试。
  - 容错性测试（Error）：通过非法数据、异常流程等错误的输入，得到预期结果。
- 如无特殊理由，测试需全覆盖。
- 每个测试用例需精确断言。
- 准备环境的代码和测试代码分离。
- 只有 Junit `Assert`，hamcrest `CoreMatchers`，Mockito 相关可以使用 static import。
- 单数据断言，应使用`assertTrue`，`assertFalse`，`assertNull`和`assertNotNull`。
- 多数据断言，应使用`assertThat`。
- 精确断言，尽量不使用`not`，`containsString`断言。
- 测试用例的真实值应名为为 actualXXX，期望值应命名为 expectedXXX。
- 测试类和`@Test`标注的方法无需 javadoc。
