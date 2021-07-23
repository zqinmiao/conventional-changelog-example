# conventional-changelog-example

## 本地仓库连接远程仓库

```
$ git remote add origin git@github.com:zqinmiao/conventional-changelog-example.git
```

## 手动增加的一级标题 Change Log

更新的 CHANGLOG.md 内容会加到一级标题之上，如下：

```
## [1.0.7](https://github.com/zqinmiao/conventional-changelog-example/compare/v1.0.6...v1.0.7) (2021-07-22)

### 🎸 Features

- 试试增加一级标题 Change Log ([4e74bef](https://github.com/zqinmiao/conventional-changelog-example/commit/4e74bef21acc2e799f0a869239ca0126c432f601))

# Change Log
```

lerna 之所以可以增加，是因为自己处理了`CHANGLOG.md`，[代码](https://github.com/lerna/lerna/blob/main/core/conventional-commits/lib/update-changelog.js#L71)如下：

```js
//...,
log.silly(type, "writing new entry: %j", newEntry);

const content = [CHANGELOG_HEADER, newEntry, changelogContents].join(
  BLANK_LINE
);

return fs.writeFile(changelogFileLoc, content.trim() + EOL).then(() => {
  log.verbose(type, "wrote", changelogFileLoc);

  return {
    logPath: changelogFileLoc,
    newEntry,
  };
});
```

如果使用[standard-version](https://github.com/conventional-changelog/standard-version) ,则自定义配置的事情就变得简单了

## changelog-config.js

可以重写`CHANGELOG.md`

用法如下:

```
$ conventional-changelog -n './changelog-config.js' -i CHANGELOG.md -s
```

## changelog-context.json

可以定义模板模板中的变量。如[conventional-changelog-writer](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-writer) 中的 [commit.hbs](https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog-writer/templates/commit.hbs)模板（看文档是这样说，但还未验证）。

用法如下:

```
$ conventional-changelog -c './changelog-context.json' -i CHANGELOG.md -s
```

## 单纯的使用`conventional-changelog -i CHANGELOG.md -s`

`CHANGELOG.md`内容会很简陋，且不会识别`# Changelog`, 如下：

```
## <small>1.1.1 (2021-07-23)</small>

* feat: 测测单纯的使用这段命令：conventional-changelog -i CHANGELOG.md -s ([165a72c](https://github.com/zqinmiao/conventional-changelog-example/commit/165a72c))


# Changelog
## [1.1.0](https://github.com/zqinmiao/conventional-changelog-example/compare/v1.0.13...v1.1.0) (2021-07-22)

```

再重新执行`standard-version`， 会重新格式化`CHANGELOG.md`，`# Changelog` 会重新回到顶部，且文档中只有它一个

## [semantic-release](https://github.com/semantic-release/semantic-release)

> semantic-release automates the whole package release workflow including: determining the next version number, generating the release notes and publishing the package.

> This removes the immediate connection between human emotions and version numbers, strictly following the [Semantic Versioning](http://semver.org/) specification.

`semantic-release`使整个包发布工作流程自动化，包括：确定下一个版本号、生成发行说明和发布包。

### [Getting started](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/getting-started.md#getting-started)

In order to use **semantic-release** you must follow these steps（为了使用**semantic-release**，您必须遵循以下步骤）:

1. [Install](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/installation.md#installation) **semantic-release** in your project
2. Configure your Continuous Integration service to [run **semantic-release**](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/ci-configuration.md#run-semantic-release-only-after-all-tests-succeeded)
3. Configure your Git repository and package manager repository [authentication](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/ci-configuration.md#authentication) in your Continuous Integration service
4. Configure **semantic-release** [options and plugins](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/configuration.md#configuration)

Alternatively those steps can be easily done with the [**semantic-release** interactive CLI](https://github.com/semantic-release/cli)（或者，可以使用 semantic-release 交互式 CLI 轻松完成这些步骤：）:

```bash
cd your-module
npx semantic-release-cli setup
```

![dialogue](https://github.com/semantic-release/semantic-release/blob/master/media/semantic-release-cli.png?raw=true)

See the [semantic-release-cli](https://github.com/semantic-release/cli#what-it-does) documentation for more details.

**Note**: only a limited number of options, CI services and plugins are currently supported by `semantic-release-cli`.
