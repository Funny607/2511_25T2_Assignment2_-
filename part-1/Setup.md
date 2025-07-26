# 1. 设置说明

本页包含有关本地设置和工作的说明。

* [1. 设置说明](#1-设置说明)

  * [1.1. 先决条件](#11-先决条件)
  * [1.2. 如何打开？](#12-如何打开)
  * [1.3. 如何运行？](#13-如何运行)
  * [1.4. 如何生成覆盖率报告？](#14-如何生成覆盖率报告)
  * [1.5. 如何测试？](#15-如何测试)
  * [1.6. 资源](#16-资源)
  * [1.7. 本地重新编译前端](#17-本地重新编译前端)

## 1.1. 先决条件

* Java JDK 17
* Gradle 8.8
* VSCode
* 你的 assignment-ii 仓库（我们假设你愿意克隆并管理它）

关于其他设置，请参见 [课程网站设置页面](https://cgi.cse.unsw.edu.au/~cs2511/25T2/setup)。

## 1.2. 如何打开？

* 你可以打开 VSCode，然后点击文件 -> 打开（在 MacOS 上为 /文件夹），选择你本地克隆的 assignment-ii 文件夹。
  确保你只打开了 assignment-ii 文件夹。
* 在 VSCode 中打开的方式：

  * `code assignment-ii`（如果在本地工作）
  * `2511 code assignment-ii`（如果在 CSE 机器上工作）

## 1.3. 如何运行？

首先要确保你打开的文件夹是正确的。确保你的资源管理器顶部显示的名称为 `assignment-ii` —— 如果不是，说明你没有打开正确的文件夹 :)

接下来，只需定位到 `App.java` 文件并点击该文件上的运行按钮。

![](/images/setup1.png)

这将同步你的前端与当前最新版本，一旦完成（应该很快），它将启动服务器。

## 1.4. 如何生成覆盖率报告？

你可以使用 `gradle coverage` 来生成覆盖率报告。这将生成可供人阅读的（html）以及更适合计算机读取的（xml）报告，扩展程序可以读取这些报告并向你显示内联覆盖率。

你可以通过 [Coverage Gutters](https://marketplace.visualstudio.com/items?itemName=ryanluker.vscode-coverage-gutters)（VSCode 的一个扩展）查看内联覆盖率报告。如下图所示。

![](/images/setup2.png)

如果你没有看到这些报告，请确保左下角的“Coverage”按钮显示类似 X% 覆盖率的内容（它仅告知当前打开文件的情况）。如果没有显示，只需点击它即可！

![](/images/setup3.png)

## 1.5. 如何测试？

你可以运行 `gradle test`，或者前往测试文件并点击图标左侧将出现的运行测试按钮。

![](/images/setup4.png)

> 如何调试测试？你只需要右键点击，然后点击“调试测试”即可。

## 1.6. 资源

要在本地运行前端，你需要将你的地牢和配置文件放置在 `src/main/resources/` 目录中。如果你希望你的资源可以通过 Gradle 和 JUnit 测试访问，那么你需要将它们放在 `src/test/resources` 中。

我们为你提供的 `FileLoader` 类将自动从正确的目录加载资源。

## 1.7. 本地重新编译前端

前端只是一些编译好的 JavaScript，以静态资源形式存放在 `main/resources/app` 文件夹中。

![](/images/setup5.png)

如果你尚未安装 Node.js，需要先安装它。

重新编译前端的步骤如下：

```
cd client # 进入 client 目录
npm install # 安装所需库
npm run build # 编译前端代码
```

你将会看到一个名为 `dist` 的文件夹（dist 是 distribution 的缩写，因为这就是你在像 AWS 这类服务中部署时使用的内容），它具有与 `resources/app` 完全相同的结构：

![](/images/setup6.png)

将 dist 文件夹中的内容复制并粘贴到 `resources/app` 中，覆盖掉之前的所有内容。

下次你本地运行前端时，它应该就包含了你的更改。
