# 约定式提交 1.0.0-beta.1

## 一、概述

约定式提交规范是一种基于提交消息的轻量级约定。
它提供了一组用于创建清晰的提交历史的简单规则；
这使得编写基于规范的自动化工具变得更容易。
这个约定与 [SemVer](https://semver.org/lang/zh-CN/) 相吻合，
在提交信息中描述新特性、bug 修复和破坏性变更等。

提交说明的规范结构如下所示：

---

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

一个简单的示例：
```
feat(module:user): 添加用户模块

[可选的 body]

Closes #123, #245, #992
```
---

提交用户名及邮箱请使用本人公司注册用户名及邮箱地址，关于 git 用户邮箱配置、SSH-Key 配置可参考 [git-常用命令 ](git-常用命令.md)

## 二、规范结构说明
### type（类型）

用于说明本次 commit 的类别，只允许使用下面标识

- feat - 新功能（feature）
- fix - 修补 bug
- docs - 文档（documentation）
- style -  格式（不影响代码运行的变动）
- refactor - 重构（即不是新增功能，也不是修改 bug 的代码变动）
- build - 修改了系统构建或者第三方依赖（比如：Maven、Gradle、Npm、gulp...）
- ci - 修改了 ci 配置文件（比如：Travis，Circle...）
- revert - 撤销以前的提交记录
- test - 增加测试
- chore - 构建过程或辅助工具的变动

注意:
如果 type 为 feat 和 fix，则该 commit 将肯定出现在 Change log 之中。其他情况（docs、chore、style、refactor、test）由你决定，要不要放入 Change log，建议是不要。

### 可选的 scope（作用域）
用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。
```shell script
git commit -m "feat(module:user):添加用户模块"
```
### subject（简短描述）
commit 目的的简短描述，不超过 50 个字符。以动词开头，使用第一人称现在时，比如 change，而不是 changed 或 changes。第一个字母小写，结尾不加句号（.）

### 可选的 body（正文）
对本次 commit 的详细描述，可以分成多行

### 可选的 footer（脚注）
- 不兼容变动
如果当前代码与上一个版本不兼容，则 Footer 部分以 BREAKING CHANGE 开头，后面是对变动的描述、以及变动理由和迁移方法
```
在可选的正文或脚注的起始位置带有 `BREAKING CHANGE:` 的提交，表示引入了破坏性 API 变更。破坏性变更可以是任意 _类型_ 提交的一部分。
```
- 关闭 Issue
如果当前 commit 针对某个 issue，那么可以在 Footer 部分关闭这个 issue (可依次关闭过个 issue`Closes #123, #245, #992`)

## 三、约定式提交规范

本文中的关键词 “必须（MUST）”、“禁止（MUST NOT）”、“必要（REQUIRED）”、“应当（SHALL）”、“不应当（SHALL NOT）”、“应该（SHOULD）”、“不应该（SHOULD NOT）”、“推荐（RECOMMENDED）”、“可以（MAY）” 和 “可选（OPTIONAL）” ，解释参考 [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) 中所述。

- 作用域字段**可以**跟随在类型字段后面。作用域**必须**是一个描述某部分代码的名词，并用圆括号包围，例如： `fix(parser):`
- 描述字段**必须**紧接在类型/作用域前缀的空格之后。描述指的是对代码变更的简短总结，例如： _fix: array parsing issue when multiple spaces were contained in string._
- 在简短描述之后，**可以**编写更长的提交正文，为代码变更提供额外的上下文信息。正文**必须**起始于描述字段结束的一个空行后。
- 在正文结束的一个空行之后，**可以**编写一行或多行脚注。脚注**必须**包含关于提交的元信息，例如：关联的合并请求、Reviewer、破坏性变更，每条元信息一行。
- 破坏性变更**必须**标示在正文区域最开始处，或脚注区域中某一行的开始。一个破坏性变更**必须**包含大写的文本 `BREAKING CHANGE`，后面紧跟冒号和空格。
- 在 `BREAKING CHANGE: ` 之后**必须**提供描述，以描述对 API 的变更。例如： _BREAKING CHANGE: environment variables now take precedence over config files._
- 工具的实现**必须不**区分大小写地解析构成约定式提交的信息单元，只有 `BREAKING CHANGE` **必须**是大写的。
- **可以**在类型/作用域前缀之后，`:` 之前，附加 `!` 字符，以进一步提醒注意破坏性变更。当有 `!` 前缀时，正文或脚注内**必须**包含 `BREAKING CHANGE: description`

## 四、为什么使用约定式提交

* 自动化生成 CHANGELOG。
* 基于提交的类型，自动决定语义化的版本变更。
* 向同事、公众与其他利益关系者传达变化的性质。
* 触发构建和部署流程。
* 让人们探索一个更加结构化的提交历史，以便降低对你的项目做出贡献的难度。

## 五、示例

### 包含了描述以及正文内有破坏性变更的提交说明
```
feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

### 包含了可选的 `!` 字符以提醒注意破坏性变更的提交说明
```
chore!: drop Node 6 from testing matrix

BREAKING CHANGE: dropping Node 6 which hits end of life in April
```

### 不包含正文的提交说明
```
docs: correct spelling of CHANGELOG
```

### 包含作用域的提交说明
```
feat(lang): add polish language
```

### 为 fix 编写的提交说明，包含（可选的） issue 编号
```
fix: correct minor typos in code

see the issue for details on the typos fixed

closes issue #12
```

## FAQ

### 在初始开发阶段我该如何处理提交说明？

我们建议你按照假设你已发布了产品那样来处理。因为通常总 *有人* 使用你的软件，即便那是你软件开发的同事们。他们会希望知道诸如修复了什么、哪里不兼容等信息。

### 提交标题中的类型是大写还是小写?

大小写都可以，但最好是一致的。

### 如果提交符合多种类型我该如何操作？

回退并尽可能创建多次提交。约定式提交的好处之一是能够促使我们做出更有组织的提交和 PR。

### 这不会阻碍快速开发和迭代吗？

它阻碍的是以杂乱无章的方式快速前进。它助你能在横跨多个项目以及和多个贡献者协作时长期地快速演进。

### 约定式提交会让开发者受限于提交的类型吗（因为他们会想着已提供的类型）？

约定式提交鼓励我们更多地使用某些类型的提交，比如 `fixes`。除此之外，约定式提交的灵活性也允许你的团队使用自己的类型，并随着时间的推移更改这些类型。

### 这和 SemVer 有什么关联呢？

`fix` 类型提交应当对应到 `PATCH` 版本。`feat` 类型提交应该对应到 `MINOR` 版本。带有 `BREAKING CHANGE` 的提交不管类型如何，都应该对应到 `MAJOR` 版本。

### 我对约定式提交做了形如 `@jameswomack/conventional-commit-spec` 的扩展，该如何版本化管理这些扩展呢？

我们推荐使用 SemVer 来发布你对于这个规范的扩展（并鼓励你创建这些扩展！）

### 如果我不小心使用了错误的提交类型，该怎么办呢？

#### 当你使用了在规范中但错误的类型时，例如将 `feat` 写成了 `fix`

在合并或发布这个错误之前，我们建议使用 `git rebase -i` 来编辑提交历史。而在发布之后，根据你使用的工具和流程不同，会有不同的清理方案。

#### 当使用了*不*在规范中的类型时，例如将 `feat` 写成了 `feet`

在最坏的场景下，即便提交没有满足约定式提交的规范，也不会是世界末日。这只意味着这个提交会被基于规范的工具错过而已。

### 所有的贡献者都需要使用约定式提交规范吗？

并不！如果你使用基于 squash 的 Git 工作流，主管维护者可以在合并时清理提交信息——这不会对普通提交者产生额外的负担。
有种常见的工作流是让 git 系统自动从 pull request 中 squash 出提交，并向主管维护者提供一份表单，用以在合并时输入合适的 git 提交信息。

## 关于

约定式提交规范受到了 [Angular 提交准则 ](https://github.com/angular/angular.js/blob/master/CONTRIBUTING.md#commit) 的启发，并在很大程度上以其为依据。

该规范的首个草案来自下面这些项目中若干贡献者们的协作：

* [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) ：一套从 git 历史中解析出约定式提交说明的工具。
* [parse-commit-message](https://github.com/olstenlarck/parse-commit-message) ：兼容规范的解析工具，可以将给定提交信息的字符串解析成对象，结果形如 `{ header: { type, scope, subject }, body, footer }`。
* [bumped](https://bumped.github.io) ：一个用于发布软件的工具，可以在为你的软件发布新版本前后轻松地执行操作。
* [unleash](https://github.com/netflix/unleash) ：一个用于自动化软件发行和发布生命周期的工具。
* [lerna](https://github.com/lerna/lerna) ：一个用于管理宏仓库（monorepo）的工具，源自 Babel 项目。

## 用于约定式提交的工具
* [jetbrains-plugin-9861-git-commit-template](https://plugins.jetbrains.com/plugin/9861-git-commit-template) ：idea git-commit-template 提交插件

![idea-git-commit-template](./_images/idea-git-commit-template.png)

* [changelog-generator](https://marketplace.visualstudio.com/items?itemName=axetroy.vscode-changelog-generator) ：vs-code 插件
* [php-commitizen](https://github.com/damianopetrungaro/php-commitizen) ：一个用于创建遵循约定式提交规范提交信息的工具。
可配置，并且可以作为 composer 依赖项用于 PHP 项目，或可在非 PHP 项目中全局使用。
* [conform](https://github.com/autonomy/conform) ：一个可用以在 git 仓库上施加配置的工具，包括约定式提交。
* [standard-version](https://github.com/conventional-changelog/standard-version) 基于 GitHub 的新 squash 按钮与推荐的约定式提交工作流，自动管理版本和 CHANGELOG。
* [commitsar](https://github.com/commitsar-app/commitsar) ： 一个检查提交信息是否符合约定式提交规范的 Go 语言工具。可在 CI 的 Docker 镜像中使用。

## 参考
- [conventionalcommits.org](https://www.conventionalcommits.org/zh-hans/v1.0.0-beta.4/)


