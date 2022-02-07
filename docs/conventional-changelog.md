# Conventional Changelog 生态探索

> 事情要从生成`CHANGELOG.md`说起。如果你想直接知道本文推荐的`生成changelog的最佳实践`，请移至文章后面[standard-version](#standard-version)部分

[Changelog?](https://speakerdeck.com/stevemao/compose-a-changelog)

对于如何编写项目的`CHANGELOG.md`这件事情上，我发现许多同学的做法还是手动维护。那么都 1202 年了，能自动的事，谁还手动呢。于是就发现了 [conventional-changelog](https://github.com/conventional-changelog)生态及工具链。

## [Conventional Changelog](https://github.com/conventional-changelog)

> Tools to generate changelogs and release notes from a project's commit messages and metadata.（从项目的提交消息和元数据生成变更日志和发布说明的工具。）

[`conventional-changelog`](https://github.com/conventional-changelog)不仅仅指的是与其同名的一个库，它是一整个生态及工具链。包括：[`conventional-changelog 同名 monorepo 包集合`](https://github.com/conventional-changelog/conventional-changelog)、[`commitlint`](https://github.com/conventional-changelog/commitlint)、[`conventional-changelog-config-spec `](https://github.com/conventional-changelog/conventional-changelog-config-spec)、[`standard-version`](https://github.com/conventional-changelog/standard-version)等。

## [同名 Conventional Changelog monorepo 仓库](https://github.com/conventional-changelog)

这是一个包含了 Conventional Changelog 核心代码、工具链、相关预设的 monorepo 仓库

开头文档处有这么两段话：

It's recommended you use the high level [standard-version](https://github.com/conventional-changelog/standard-version) library, which is a drop-in replacement for npm's version command, handling automated version bumping, tagging and CHANGELOG generation.（建议您使用[standard-version](https://github.com/conventional-changelog/standard-version)，它是 `npm's version` 的直接替代品，处理自动版本更新、标签和 CHANGELOG 生成。）

Alternatively, if you'd like to move towards completely automating your release process as an output from CI/CD, consider using [semantic-release](https://github.com/semantic-release/semantic-release).（或者，如果您想完全自动化您的发布过程，作为 CI/CD 的输出，请考虑使用[semantic-release](https://github.com/semantic-release/semantic-release)）

`standard-version` 和 `semantic-release` 后面会说到，我们接下来先探索`Conventional Changelog`

### Conventional Changelog 相关的插件

- [grunt](https://github.com/btford/grunt-conventional-changelog)
- [gulp](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/gulp-conventional-changelog)
- [atom](https://github.com/conventional-changelog/atom-conventional-changelog)
- [vscode](https://github.com/axetroy/vscode-changelog-generator)

### Conventional Changelog 生态相关的核心库

- [conventional-changelog-cli](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli) - the full-featured command line interface（功能齐全的 CLI）

- [standard-changelog](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/standard-changelog) - command line interface for the angular commit format.（angular 提交格式的 CLI）
- [conventional-github-releaser](https://github.com/conventional-changelog/conventional-github-releaser) - Make a new GitHub release from git metadata（从 git 元数据创建一个新的 GitHub release）
- [conventional-recommended-bump](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-recommended-bump) - Get a recommended version bump based on conventional commits（根据常规提交获得推荐的版本变更）
- [conventional-commits-detector](https://github.com/conventional-changelog/conventional-commits-detector) - Detect what commit message convention your repository is using（对存储库使用的 commit 消息约定进行检查）
- [commitizen](https://github.com/commitizen/cz-cli) - Simple commit conventions for internet citizens.（简约的 commit 提交约定）
- [commitlint](https://github.com/conventional-changelog/commitlint) - Lint commit messages（commit 消息检查）

## [conventional-changelog-cli](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli)

[![NPM version](https://camo.githubusercontent.com/098368890fdeaa1cb6f47b14fa4edc01d0a99ce6e8769ec2d33c12589703da2c/68747470733a2f2f62616467652e667572792e696f2f6a732f636f6e76656e74696f6e616c2d6368616e67656c6f672d636c692e737667)](https://www.npmjs.com/package/conventional-changelog-cli)

> 我们先从 conventional-changelog-cli 开始探索

conventional-changelog-cli 可以帮我们生成 CHANGELOG.md，用法如下：

```
$ npm install -g conventional-changelog-cli
$ cd my-project
$ conventional-changelog -p angular -i CHANGELOG.md -s
```

那么，上面的命令参数，如：`-p`、`-i`、`-s`，是什么意思呢？

```
# 终端中可以--help，进行查看
$ npx conventional-changelog --help

Generate a changelog from git metadata

  Usage
    conventional-changelog

  Example
    conventional-changelog -i CHANGELOG.md --same-file

  Options
    -i, --infile              Read the CHANGELOG from this file

    -o, --outfile             Write the CHANGELOG to this file
                              If unspecified, it prints to stdout

    -s, --same-file           Outputting to the infile so you don't need to specify the same file as outfile

    -p, --preset              Name of the preset you want to use. Must be one of the following:
                              angular, atom, codemirror, ember, eslint, express, jquery, jscs or jshint

    -k, --pkg                 A filepath of where your package.json is located
                              Default is the closest package.json from cwd

    -a, --append              Should the newer release be appended to the older release
                              Default: false

    -r, --release-count       How many releases to be generated from the latest
                              If 0, the whole changelog will be regenerated and the outfile will be overwritten
                              Default: 1

    --skip-unstable           If given, unstable tags will be skipped, e.g., x.x.x-alpha.1, x.x.x-rc.2

    -u, --output-unreleased   Output unreleased changelog

    -v, --verbose             Verbose output. Use this for debugging
                              Default: false

    -n, --config              A filepath of your config script
                              Example of a config script: https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog-cli/test/fixtures/config.js

    -c, --context             A filepath of a json that is used to define template variables
    -l, --lerna-package       Generate a changelog for a specific lerna package (:pkg-name@1.0.0)
    -t, --tag-prefix          Tag prefix to consider when reading the tags
    --commit-path             Generate a changelog scoped to a specific directory
```

### 稍微看下源码

我们通过查看[conventional-changelog-cli 源码](https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog-cli/cli.js)，可以看到：它依赖于「`conventional-changelog`」下的 [「conventional-changelog」](https://github.com/conventional-changelog/conventional-changelog/blob/d7477814399c8687ba705d03dea28f8648c0c855/packages/conventional-changelog/index.js#L6) :

```
#!/usr/bin/env node
'use strict'

//...
const conventionalChangelog = require('conventional-changelog')

```

而「`conventional-changelog`」下的 `conventional-changelog/index.js`的`conventionalChangelog` 方法又依赖于[「conventional-changelog-core」](https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog-core/README.md)的[`conventional-changelog-core/index.js`](https://github.com/conventional-changelog/conventional-changelog/blob/d7477814399c8687ba705d03dea28f8648c0c855/packages/conventional-changelog-core/index.js#L13)

### [conventional-changelog-core](https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog-core/README.md)

既然上面我们找到了`conventional-changelog-core`，那么我们就来说道说道。

conventional-changelog-core 是 conventional-changelog 相关工具链的核心库。那么生成 changelog 的原理简单说来就是：利用[git-raw-commits](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/git-raw-commits)拿到`git log`中的 commit 信息，然后经过一系列再加工，最终输出为 changelog 的内容。

### [conventional-changelog-cli 推荐的工作流](ges/conventional-changelog-cli/README.md#recommended-workflow)

1. Make changes
2. Commit those changes
3. Make sure Travis turns green
4. Bump version in package.json
5. conventionalChangelog
6. Commit package.json and CHANGELOG.md files
7. Tag
8. Push

### [结合 npm version](https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog-cli/README.md#with-npm-version)

可以结合 npm scripts 的 hooks 做一些额外的操作

```json
{
  "scripts": {
    "version": "conventional-changelog -p angular -i CHANGELOG.md -s && git add CHANGELOG.md",
    "postversion": "git push && git push origin --tags"
  }
}
```

此时推荐的工作流：

1. Make changes
2. Commit those changes
3. Pull all the tags
4. Run the [`npm version [patch|minor|major]`](https://docs.npmjs.com/cli/v7/commands/npm-version) command
5. Push

你可以选择添加一个 `preversion` 脚本来打包您的项目或运行全套测试。还有一个 `postversion` 脚本来清理你的项目并推送你的版本和标签。

#### 跟上面推荐的工作流做一遍

```
# 建一个示例文件夹，并初始化git 和 npm
$ mkdir conventional-changelog-example
$ cd conventional-changelog-example
$ touch README.md
$ git init
$ npm init

# 安装 conventional-changelog-cli
$ npm i conventional-changelog-cli -D

# 执行git 操作
$ git add .
$ git commit -m "feat: 初始化项目"
$ npm version patch
```

- 此时 package.json 中的版本号，已经由 1.0.0 更新到 1.0.1 了
- 项目根目录也自动生成了`CHANGELOG.md`，内容如下：

  > 通常 CHANGELOG.md 中每一个 commit 信息应当要带上对应的链接的。这里没有，是因为还未设置远程仓库的缘故

  ```
  ## 1.0.1 (2021-07-22)

  ### Features

  * 初始化项目 af265a9
  ```

- git tag 也已经存在

  ```
  $ git tag -l

  v1.0.1
  ```

- git commit 信息如下：

  ```
  $ git log

  commit fd0ea88b8976c57d527130b03cd6e27a979a8ba4 (HEAD -> master, tag: v1.0.1)
  Author: xxx <xxx@gmail.com>
  Date:   Thu Jul 22 13:40:33 2021 +0800

      1.0.1

  commit af265a9c89bed5e8cd961e4becff50273b586889
  Author: xxx <xxx@gmail.com>
  Date:   Wed Jul 21 23:17:47 2021 +0800

      feat: 初始化项目
  ```

- 此刻 tag 的信息

  ```
  $ git show v1.0.1

  tag v1.0.1
  Tagger: xxx <xxx@gmail.com>
  Date:   Thu Jul 22 13:40:33 2021 +0800

  1.0.1

  ```

#### 那么，如果我想要更改 tag 的信息，应该怎么做呢？

那么，我们就需要借助`.npmrc`及其配置了，配置如下：

```
# tag 版本号的前缀
tag-version-prefix=""

# tag 信息的模板
# 「%s」是版本号
# 「:tada:」 是 emoji 表情 🎉
message="chore(release): %s :tada:"
```

> PS：emoji 表情`:tada:`有可能不生效，可以直接使用「🎉」

然后把之前的工作流的流程再走一遍。然后看看此刻的 tag 信息（瞧，成了）：

```
$ git tag -l

1.0.1

$ git show 1.0.1

tag 1.0.1
Tagger: xxx <xxx@gmail.com>
Date:   Thu Jul 22 13:56:32 2021 +0800

chore(release): 1.0.1 :tada:
```

### 自定义`CHANGELOG.md`的内容

现在自动生成`CHANGELOG.md`及其内容的目标我们已经达成，那么怎么自定义它的内容呢？比如：小标题`Features` 我想加上一个表情。

从[conventionalChangelogCore API](https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog-core/README.md#conventionalchangelogcoreoptions-context-gitrawcommitsopts-parseropts-writeropts)我们得知，其实重写相关的逻辑是由这个库：[conventional-changelog-writer](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-writer)来完成的。看了一圈下来，源码阅读性差了点，理解起来其实有点慢。倒不如去看看一些预设的库，它们的配置在阅读性上面还不错。

**如之前我们执行的命令中的`-p angular`，就是用的`angular`的预设配置：**

```
conventional-changelog -p angular -i CHANGELOG.md -s && git add CHANGELOG.md
```

#### [conventional-changelog-angular](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-angular)

查看`conventional-changelog-angular`的源码，发现有一个[`writer-opts.js`](https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog-angular/writer-opts.js),里面`getWriterOpts`方法如下：

```javascript
function getWriterOpts() {
  return {
    transform: (commit, context) => {
      //...,

      if (commit.type === "feat") {
        commit.type = "Features";
      } else if (commit.type === "fix") {
        commit.type = "Bug Fixes";
      } else if (commit.type === "perf") {
        commit.type = "Performance Improvements";
      } else if (commit.type === "revert" || commit.revert) {
        commit.type = "Reverts";
      } else if (discard) {
        return;
      } else if (commit.type === "docs") {
        commit.type = "Documentation";
      } else if (commit.type === "style") {
        commit.type = "Styles";
      } else if (commit.type === "refactor") {
        commit.type = "Code Refactoring";
      } else if (commit.type === "test") {
        commit.type = "Tests";
      } else if (commit.type === "build") {
        commit.type = "Build System";
      } else if (commit.type === "ci") {
        commit.type = "Continuous Integration";
      }

      //...,

      return commit;
    },
  };
}
```

这段代码就是重写 changelog 内容的。

#### 自定义配置`changelog-config.js`

```
$ touch changelog-config.js
```

changelog-config.js

```js
module.exports = {
  writerOpts: {
    transform: (commit, context) => {
      //...,

      if (commit.type === "feat") {
        // 这里稍微改动一下
        commit.type = "🎸 Features";
      }

      return commit;
    },
  },
};
```

执行一下命令，然后你会看到改动生效了

```
$ conventional-changelog -p angular -n './changelog-config.js' -i CHANGELOG.md -s
```

到这里，我们对`Conventional Changelog`有了大致的了解，也应该知道了一个符合规范的版本管理工作流应该怎么进行了。

## [commitlint](https://github.com/conventional-changelog/commitlint)

[![npm version](https://camo.githubusercontent.com/ea7c2aeeb927281a023217e14768c911b25cc517620861d9be1248642121d6e8/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f40636f6d6d69746c696e742f636c692e7376673f7374796c653d666c61742d737175617265)](https://npmjs.org/package/@commitlint/cli)

`commitlint` 是对 commit 信息进行检查的工具。具体详细用法可以直接查看 github 上的文档，讲的很仔细，这里就不再赘述。

在使用时，建议使用常见类型配置[`@commitlint/config-conventional`](https://github.com/conventional-changelog/commitlint/tree/master/@commitlint/config-conventional)

### 安装使用

```bash
# Install commitlint cli and conventional config
npm install --save-dev @commitlint/{config-conventional,cli}
# For Windows:
npm install --save-dev @commitlint/config-conventional @commitlint/cli

# Configure commitlint to use conventional config
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

`commitlint.config.js`

```js
module.exports = {
  extends: ["@commitlint/config-conventional"],
  rules: {
    // 这里可以自定义规则对继承的配置进行覆盖
    //...,
  },
};
```

在 commit 创建之前对提交进行 lint，可以使用 Husky 的 commit-msg 钩子：

```bash
# Install Husky v6
npm install husky --save-dev
# or
yarn add husky --dev

# Activate hooks
npx husky install
# or
yarn husky install

# Add hook
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
# Sometimes above command doesn't work in some command interpreters
# You can try other commands below to write npx --no -- commitlint --edit $1
# in the commit-msg file.
npx husky add .husky/commit-msg \"npx --no -- commitlint --edit '$1'\"
# or
npx husky add .husky/commit-msg "npx --no -- commitlint --edit $1"

# or
yarn husky add .husky/commit-msg 'yarn commitlint --edit $1'
```

查看 [husky 文档](https://typicode.github.io/husky/#/?id=manual) ，了解如何在安装后为不同的 `yarn` 版本自动启用 Git hooks。

## [commitizen](https://github.com/commitizen/cz-cli)

[![npm version](https://camo.githubusercontent.com/fb25c43afae8e2e05c7d2f7f5b49d346cfd29bf65c2f89e65f359039b0404efd/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f636f6d6d6974697a656e2e737667)](https://www.npmjs.com/package/commitizen)

commitizen 是一个命令行交互式的 git commit 替代工具。可以让你不再手动输入 commit 类型。感兴趣的可以试试看，具体用法大致如下：

```
$ npx git-cz
npx: installed 1 in 2.997s
? Select the type of change that you're committing:
  🎡  ci:         CI related changes
  ⚡️  perf:       A code change that improves performance
  💍  test:       Adding missing tests
❯ 🎸  feat:       A new feature
  🐛  fix:        A bug fix
  🤖  chore:      Build process or auxiliary tool changes
  ✏️  docs:       Documentation only changes
(Move up and down to reveal more choices)
```

选完类型后的一系列操作

```bash
? Select the type of change that you're committing: ✏️  docs:       Documentation only changes
? Write a short, imperative mood description of the change:
  [-------------------------------------------------------------] 55 chars left
   docs: 更新docs
? Provide a longer description of the change:
  更新了docs下的md
? List any breaking changes
  BREAKING CHANGE:
? Issues this commit closes, e.g #123:
[master 9430537] docs: ✏️ 更新docs
 1 file changed, 553 insertions(+)
```

查看生成的 commit 信息

```bash
$ git log

commit 94305379b7bc6870c7bef668532e788abde77ba9 (HEAD -> master)
Author: xxx <xxx@gmail.com>
Date:   Mon Feb 7 15:11:50 2022 +0800

    docs: ✏️ 更新docs

    更新了docs下的md
```

## [Conventional Changelog Configuration Spec](https://github.com/conventional-changelog/conventional-changelog-config-spec)

[conventional-changelog-config-spec](https://github.com/conventional-changelog/conventional-changelog-config-spec) 是 Conventional Changelog 的配置规范，相关的工具是按此规范进行工作的。

比如哪些`commit 类型`会被写入 changelog 的代码：

从[这里](https://github.com/conventional-changelog/conventional-changelog/blob/d747781439/packages/conventional-changelog-writer/index.js#L75) 到 [这里](https://github.com/conventional-changelog/conventional-changelog/blob/d7477814399c8687ba705d03dea28f8648c0c855/packages/conventional-changelog-conventionalcommits/writer-opts.js#L179) 相关的代码可以窥见一斑

```js
function defaultConfig(config) {
  config = config || {};
  config.types = config.types || [
    { type: "feat", section: "Features" },
    { type: "feature", section: "Features" },
    { type: "fix", section: "Bug Fixes" },
    { type: "perf", section: "Performance Improvements" },
    { type: "revert", section: "Reverts" },
    { type: "docs", section: "Documentation", hidden: true },
    { type: "style", section: "Styles", hidden: true },
    { type: "chore", section: "Miscellaneous Chores", hidden: true },
    { type: "refactor", section: "Code Refactoring", hidden: true },
    { type: "test", section: "Tests", hidden: true },
    { type: "build", section: "Build System", hidden: true },
    { type: "ci", section: "Continuous Integration", hidden: true },
  ];
  config.issueUrlFormat =
    config.issueUrlFormat || "{{host}}/{{owner}}/{{repository}}/issues/{{id}}";
  config.commitUrlFormat =
    config.commitUrlFormat ||
    "{{host}}/{{owner}}/{{repository}}/commit/{{hash}}";
  config.compareUrlFormat =
    config.compareUrlFormat ||
    "{{host}}/{{owner}}/{{repository}}/compare/{{previousTag}}...{{currentTag}}";
  config.userUrlFormat = config.userUrlFormat || "{{host}}/{{user}}";
  config.issuePrefixes = config.issuePrefixes || ["#"];

  return config;
}
```

### 因为规范的存在，所以自定义 changelog 内容并不是完全自由的

比如：如果我想在`CHANGELOG.md`头部加个`# Change Log`的头，单纯的使用`conventional-changelog`的话是做不到的。

不过像[`lerna`](https://github.com/lerna/lerna) 和 [`standard-version`](https://github.com/conventional-changelog/standard-version) 它们都是最后重写`CHANGELOG.md`实现的。具体细节感兴趣的话可以去看它们的源码。

<h2 id="standard-version">standard-version</h2>

[![npm version](https://camo.githubusercontent.com/951c0d61f5f547cc8002548fd930e1fd53aacf9221e3f0f401c16b42d2915793/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f7374616e646172642d76657273696f6e2e737667)](https://www.npmjs.com/package/standard-version)

[standard-version](https://github.com/conventional-changelog/standard-version) 简而言之就是：`npm version` 与 `Conventional Changelog`的结合体。

- 遵循[ Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/) 的规范

  > 约定式提交规范是一种基于提交信息的轻量级约定。 它提供了一组简单规则来创建清晰的提交历史； 这更有利于编写自动化工具。 通过在提交信息中描述功能、修复和破坏性变更， 使这种惯例与 [SemVer](http://semver.org/) 相互对应。（我们之前讲到的[conventional-changelog-config-spec](https://github.com/conventional-changelog/conventional-changelog-config-spec/)也是基于`Conventional Commits Specification`规范）

- 可以使用`.versionrc`, `.versionrc.json` or `.versionrc.js` 为配置文件，基于[conventional-changelog](https://github.com/conventional-changelog/conventional-changelog)生成`changelog`（所以其配置默认与[conventional-changelog-config-spec](https://github.com/conventional-changelog/conventional-changelog-config-spec/)规范一致）

### `.versionrc.js`配置

```js
module.exports = {
  // 自定义CHANGELOG.md的头部标题
  header: "# Changelog",
  // 自定义release tag 的信息
  releaseCommitMessageFormat: "chore(release): v{{currentTag}} :tada:",
  /**
   * 自定义commit类型
   * 可以定义有哪些类型；
   * section：控制类型在CHANGELOG.md中的标题是什么
   * hidden：是否将此类型写入CHANGELOG.md
   */
  types: [
    { type: "feat", section: "🎸 Features" },
    { type: "feature", section: "Features" },
    { type: "fix", section: "Bug Fixes" },
    { type: "perf", section: "Performance Improvements" },
    { type: "revert", section: "Reverts" },
    { type: "docs", section: "Documentation" },
    { type: "style", section: "Styles", hidden: true },
    { type: "chore", section: "Miscellaneous Chores", hidden: true },
    { type: "refactor", section: "Code Refactoring", hidden: true },
    { type: "test", section: "Tests", hidden: true },
    { type: "build", section: "Build System", hidden: true },
    { type: "ci", section: "Continuous Integration", hidden: true },
  ],
};
```

### 那么我们此时的工作流应当是：

1. Make changes
2. Commit those changes
3. Make sure Travis turns green
4. npx standard-version
5. Run `git push --follow-tags origin master && npm publish` to publish

> `npm publish` 可以结合 `prepublishOnly`、`postpublish` 做些额外的操作

到这里，如果我们仅仅生成`CHANGELOG.md`的话（不做更加自由度的定制），使用`standard-version`反而是更佳的方式，也是官方推荐的用法。

### [semantic-release](https://github.com/semantic-release/semantic-release)

文章开头我们有提到`semantic-release`

#### [How is `standard-version` different from `semantic-release`?（`standard-version`和`semantic-release`有什么不同）](https://github.com/conventional-changelog/standard-version#how-is-standard-version-different-from-semantic-release)

[semantic-release](https://github.com/semantic-release/semantic-release) is described as:

> semantic-release automates the whole package release workflow including: determining the next version number, generating the release notes and publishing the package.

While both are based on the same foundation of structured commit messages, standard-version takes a different approach by handling versioning, changelog generation, and git tagging for you without automatic pushing (to GitHub) or publishing (to an npm registry). Use of standard-version only affects your local git repo - it doesn't affect remote resources at all. After you run standard-version, you can review your release state, correct mistakes and follow the release strategy that makes the most sense for your codebase.

We think they are both fantastic tools, and we encourage folks to use semantic-release instead of standard-version if it makes sense for their use-case.

上面的文字阐述了它们之间的不同。简言之：如果你有一个 CI/CD 的服务，那么`semantic-release` 的自动版本更新、推送及发布则很适合。`standard-version` 只影响你本地的 git 仓库，你可以查看发布状态、纠正错误并遵循最适合您的代码库的发布策略。

更多关于`semantic-release`的使用和集成，可移步 github 有详细介绍，这里就不再复述。

## 结语

Conventional Changelog 生态探索到这里就结束了，有了大致的了解和梳理。对于单一存储库来说，`standard-version`还是比较香的。但是对于 monorepo 仓库来讲，业界也有更好的方案，如[`changesets`](https://github.com/changesets/changesets)。

随着时代的发展，前端工具链的更新也是日新月异。对不同生态的梳理，是学习也是记录，至少证明我曾经来过。写于 2021 年忘了是哪个月来着～

## 相关文档

[gitmoji（用于 git commit 信息的 emoji 表情指南）](https://github.com/carloscuesta/gitmoji)
