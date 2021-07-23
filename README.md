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
