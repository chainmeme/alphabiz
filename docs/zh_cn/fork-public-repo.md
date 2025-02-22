# 公共仓库版本开发指南

公共仓库地址: [alphabiz](https://github.com/tanshuai/alphabiz)

## 开发前的准备

请参考 <a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/prepare-before-dev.md#prepare-before-dev">此文档</a> 进行开发前的准备。

## 开发流程步骤

### 1.Fork 公共仓库

请参考 <a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#fork-repo">此文档</a> 进行 Fork 操作。

### 2.同步源仓库最新代码

如果源仓库没有更新，则可以跳过此步骤。如需同步，请参考 <a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#sync-upstream-repo">此文档</a> 。

### 3.克隆新创建的仓库到本地仓库

请参考 <a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#clone-repo">此文档</a> 进行克隆操作。

### 4.安装本仓库的 Node.js 模块

在仓库根目录打开命令行，并执行以下命令：

```bash
yarn
```

如果遇到安装报错，请参考 [此文档](https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/development-issues-solutions.md#yarn-install-error)

### 5.安装 unpackaged 文件的 Node.js 模块

由于公共仓库的代码是私有仓库预编译过的代码，需要安装预编译代码的模块，在仓库根目录打开命令行，并执行以下命令：

```bash
yarn unpackaged
```

### 6.定制 app

请使用记事本或[代码编辑器](https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#code-editor)工具，修改`developer/`配置文件。详细定制化配置信息请参考 <a href="https://github.com/zeeis/customization-test/blob/main/docs/zh_cn/customized-content.md#customization">此文档</a> 。

### 7.构建 app

在仓库根目录打开命令行，并执行以下命令：
```bash
yarn packager
```
注意以下内容：
- 生成的 app 存储路径为`build/electron/[app名称]-[系统名称]`
- 暂定调试流程:
  1. 重复执行步骤 7 和 8，检查新生成的 app。
  2. 修改配置后，开启 e2e 测试的 debug 模式并查看配置修改情况。可以通过 ctrl+c 关闭运行的命令行或关闭运行中的 electron 和 playwright 窗口，并重复操作 `yarn test:e2e:electron:custom --debug`
- 如需修改构建 app 安装包的配置，请参考 [此文档](https://github.com/zeeis/customization-test/blob/main/docs/zh_cn/customized-content.md#install-package-config)。

### 8.生成安装包
在仓库根目录打开命令行，并执行以下命令：
```bash
yarn make
```
注意以下内容：
- 安装包存储路径为`out/installers/[app版本号]`
- 如果需要在 Windows 上生成安装包，请先安装<a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#wix-tool">Wix Toolset</a>
- 如果在`yarn make`过程中使用 Ctrl+C 强制退出，可能导致部分动态修改的文件无法恢复。如遇到此问题，请参考 <a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#template">这里</a> 解决。
- 在 Windows 系统下，可能会因为 Windows 路径长度限制而导致 yarn make 报错，提示某个文件路径过长。如遇到此问题，请参考 <a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/development-issues-solutions.md#path-to-long">这里</a> 解除路径长度限制。

### 9.编译其他版本App

请参考 <a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/build-app.md">此文档</a> 编译其他版本的 App（如 snap、amd64、universal 版本的 dmg 安装包、Android、iOS 等）。
### 10.将安装包发布到 GitHub Releases 上请参考 <a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#github-release">此文档</a>

## 测试 & 自动发布

1. 公共仓库的测试用例位于`test/`文件夹下，私有仓库的测试用例位于`test-secret/`文件夹下。
2. 运行测试用例需要配置环境变量，请参考<a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#test-env">此文档</a>
3. `test/`文件夹中的测试用例是从私有仓库同步的。如果要编写自己的测试用例，请建立一个新文件夹以避免冲突。
4. 如果测试失败，需要删除测试环境，请在命令行中输入以下命令：
```bash
yarn node copy-patch.js --post
```
5. 每次提交代码时都会运行测试用例，每晚会执行用于发布 nightly 版本的工作流。具体内容请参考[这里](https://github.com/tanshuai/alphabiz/blob/main/.github/workflows/push.yml)。
6. 如果工作流中包含自动提交 commit 的步骤，建议统一设置 Actions Runner 的时区为东八区，以方便管理版本号中的日期。具体设置方法请参考<a href="https://github.com/zeeis/customization-test/tree/main/docs/zh_cn/fork-repo-hint.md#set-timezone">此文档</a>