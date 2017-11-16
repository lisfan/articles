# ! NPM 学习笔记

@(002_前端开发)[node, node package manager, 学习笔记, 包管理器]

---

**修订历史**

| 修订版本 | 修订时间 | 修订人 | 描述 |
| ----- | ---------- | ------ | --- |
| 1.0.0 | 2017-09-15~2017-11-14 | lisfan | 开篇 |

| 版本 | 学习深度 | 学习进度 | 学习范围 |
| ------ | --- | --- | ------- |
| v5.4.2 | 90% | 90% | 官方文档 |


## 目录

[TOC]

----

## 问题列表

- [ ]npm-team

## 参考文章

[npm官方网站 EN](https://www.npmjs.com/)
[npm官方文档 EN](https://docs.npmjs.com/)
[npm起步 EN](https://docs.npmjs.com/getting-started)
[npm工作原理 EN](https://docs.npmjs.com/how-npm-works/packages)
[npm配置选项 npm-config EN](https://docs.npmjs.com/misc/config)
[npm配置选项 npm-config 详解 CN](https://github.com/liangklfangl/npm-command)
[npm配置选项 npm-config 详解 CN](http://blog.csdn.net/zmrdlb/article/details/53187804)
[npm标签管理 dist-tag CN](http://blog.csdn.net/zmrdlb/article/details/53187804)
[npm脚本钩子npm-script EN](https://docs.npmjs.com/misc/scripts)
[npm脚本钩子npm-script CN](http://blog.csdn.net/qq_24122593/article/details/53138620)

## 学习大纲

- [x]了解`NPM`的作用
- [x]了解`.npmrc`的配置
- [x]理解`package.json`文件各配置参数的作用
- [x]了解并记忆常用`CLI`指令
- [x]如何对npm包进行下载/删除等操作
- [x]如何发布、管理npm包
- [x]再回顾翻阅一次npm官方文档（谷歌翻译成中文）
- [x]package-lock.json
- [x]npm-floders
- [x]npm org
- [x]cli commands
- [ ]bin 命令的开发
- [ ]node-gyp

## 产出成果

- [ ].npmrc 配置（项目级别）
- [ ].npmrc 配置（全局级别）
- [ ]npm init 时的 package.json 模板 /Users/lisfan/.npm-init.js
- [ ]npm命令速记清单
- [ ]配置常用script
- [ ]一份.npmignore或者.gitignore

## 第三方扩展

## 基础
### NPM

**Build amazing things**

`npm` 是一个包管理工具，且是世界最大的应用资源库，是一种强大的新方式去发现可重用的代码包并组织他们
> npm is the package manager for JavaScript and the world’s largest software registry. Discover packages of reusable code — and assemble them in powerful new ways.

使用`npm`安装，分享，贡献代码；在你的项目中管理依赖；和分享和接收反馈等等
> Use npm to install, share, and distribute code; manage dependencies in your projects; and share & receive feedback with others.

包只是一个包含一个或多个文件的目录，以及一个名为**package.json**的文件，其中包含有关该的元数据。一个典型的应用程序，如网站，将取决于几十个或数百个包。这些包往往很小; 一般的建议是创建一个小的块构建来解决一个问题。这使您可以从这些小型构建块中构建更大的定制解决方案。

### 版本

默认情况下 `npm` 的版本会随着 `node` 的升级而升级

`npm` 作为一个独立的项目，你也可以主动升级

```bash
# 安装最新版本
npm install npm@latest -g

# 卸载
npm uninstall npm -g
```

**node 版本管理/自由切换工具**
- n
- nave
- nodist
- nvm

###  包

包含以下任意一种的，都可以称呼为`package`
- 一个含了一个`package.json`文件的文件夹 - a folder containing a program described by a package.json file
- 一个`gzip`压缩包 - a gzipped tarball containing (a)
- 一个用于关联该包的url地址 - a url that resolves to (b)
- 一个发布到源上的地址 - a `<name>@<version>` that is published on the registry with (c)
- a `<name>@<tag>` that points to (d)
- a `<name>` that has a latest tag satisfying (e)
- a git url that, when cloned, results in (a).

###  模块

大多数包都是模块，模块能被`node`程序的`require()`方法加载，包含以下所有内容才可以被称为模块
- 目录下存在`package.json`文件包含`main`字段
- 目录下存在`index.js` 
- 一个`JavaScript`文件

其中一些包，例如`CLI`包，仅包含可执行的命令行交互，并不提供`main`字段，所以它们**不是模块**

### 工作原理

**npm@v2**存在依赖地狱问题，为了包依赖模块之间不产生冲突，会在各自的包目录下嵌套安装其依赖
**npm@v3**相对**npm@v2** 管理**node_module**目录的方式做了优化，会尝试减少深层嵌套造成的冗余，包将以平辅展开方式放至在**node_module**下，只有当包依赖模块版本之间产生冲突时，才会在自身包目录下安装相关的依赖模块
依赖包的版本受安装顺序的影响，先安装的依赖模块将lfh

### 权限

在使用过程中会碰到如下的权限问题
- Change the permission to npm's default directory.
- Change npm's default directory to another directory.
- Install node with a package manager that takes care of this for you.

**npm -g 权限移除**

```bash
 sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```

若无效，请查阅https://docs.npmjs.com/getting-started/fixing-npm-permissions

### 依赖包安装方式

有如下两种方式
- 本地安装（默认）：将当前依赖包安装在当前包的根目录下的**node_modules**文件夹里 - Local install (default): puts stuff in ./node_modules of the current package root.
- 全局安装：将依赖包安装到全局**node_modules**文件夹下 - Global install (with -g): puts stuff in /usr/local or wherever node is installed.

根据使用场景进行选择对应的安装方式
- 如果你只想加载依赖包，使用本地安装方式 - Install it locally if you're going to require() it.
- 如果你想运行命令行，使用全局安装方式 - Install it globally if you're going to run it on the command line.
- 如果你两者都需要，可以两处都安装，或者使用`npm link` - If you need both, then install it in both places, or use npm link.


### 作用域包 npm-scope

`scope`就像npm包的命名空间，如果名称中以`@`开始，则它是一个作用域包，如`@scope/project-name`，作用域包可以设置为私有或公共

- 发布公共作用域包到主要的npm源，初次发布的时候必须指定`--access publish`
- 发布私有作用域包，你必须有npm的私有模块账号(收费)，发布时指定`--access restricted`

你可以通过npm-config设置作用域自已的配置

```bash
npm config set @myco:registry http://reg.example.com
````

一个作用域与注册源关联后
- 执行`npm install`时，该作用域的部都将从该注册源请求安装
- 执行`npm publish`时，该作用域的部都将从该注册源请求安装
- 任何 npm publish包含范围的包名称将被发布到该注册表。


### 语义化版本

[语义化版本规范 2.0.0](http://semver.org/lang/zh-CN/)

语义化版本规则，由**主版本号(Major).次版本号(Minor).补丁版本号(Patch)-其他信息**构成（如1.2.0），遵循如下规则：
- bug修复和小的变更：发布**修订版本号**，递增末位数字，如1.0.1 - Bug fixes and other minor changes: Patch release, increment the last number, e.g. 1.0.1
- 新的特性并且不会破坏已存在的特性：发布**次版本**号，递增中间数字，如1.1.0 - New features which don't break existing features: Minor release, increment the middle number, e.g. 1.1.0
- 产生变化且无法向后兼容的：发布**主版本号**，递增首位数字，如2.0.0 - Changes which break backwards compatibility: Major release, increment the first number, e.g. 2.0.0
- 先行版本号及版本编译信息可以加到“主版本号.次版本号.修订号”的后面，作为延伸。

`npm`使用语义化版本管理依赖包的版本下载范围

```
Local Paths ：本地路径引用
version ：完全匹配
>version ：大于
>=version ：大于或等于
<version ：小于
<=version ：小于或等于
>=version1 <=version2 ： 范围，大于且小于
range1 || range2 ： 多个范围
~version ： 只在修订版本号变化
^version ： 只在次版本号变化
1.2.x ： 1.2的任意修订版本
* ： 任意版本
"" ： 任意版本
tag ： 匹配到某个tag版本
URLs as Dependencies ：使用一个tar包的url地址作为依赖
Git URLs as Dependencies ：使用git地址作为依赖包，支持git, git+ssh, git+http, git+https, or git+file，未指定版本时，默认使用master分支
GitHub URLs ：
```

**example**
```
{ "dependencies" :
  { "foo" : "1.0.0 - 2.9999.9999",
    "bar" : ">=1.0.2 <2.1.2",
    "baz" : ">1.0.2 <=2.3.4",
    "boo" : "2.0.1",
    "qux" : "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0",
    "asd" : "http://asdf.com/asdf.tar.gz",
    "til" : "~1.2",
    "elf" : "~1.2.3",
    "two" : "2.x",
    "thr" : "3.3.x",
    "lat" : "latest",
    "dyl" : "file:../dyl",
  }
}
```

### 发布目标版本 dist-tag 

标签是用于组织和标记不同版本的包，作为语义版本的补充。除了更易于阅读之外，标签还允许发布商更有效地分发其包。
> Tags are a supplement to semver (e.g., v0.12) for organizing and labeling different versions of packages. In addition to being more human-readable, tags allow publishers to distribute their packages more effectively.

默认情况下，`npm publish`将使用`latest`标签标记您的包。可以使用`--tag`参数，指定要使用的其他标签。
> By default, npm publish will tag your package with the latest tag. If you use the --tag flag, you can specify another tag to use. For example, the following will publish your package with the beta tag:

> npm publish --tag beta
> 使用标签进行标记

> 默认情况下`npm install <pkg>`会使用安装`latest`版本。

> npm install `<pkg>@<tag>`
> 安装指定版本

> 因为`dist-tag`与**语义版本**共享相同的命名空间，避免使用任何可能导致冲突的标签名称。最佳做法是**避免使用以数字或字母“v”开头的标签**。
> Because dist-tags share the same namespace with semver, avoid using any tag names that may cause a conflict. The best practice is to avoid using tags beginning with a number or the letter "v".

### 团队协作 npm-orgs

团队用户有3个级别
1. 超级管理员(一人)：管理清单和增加用户到组里
2. 团队管理员：管理组员权限和包访问权限
3. 开发者：基于提供的访问权限工作

> There are three levels of org users:
1. Super admin, controls billing & adding people to the org.
2. Team admin, manages team membership & package access.
3. Developer, works on packages they are given access to.

超级管理员只有一个人，它可以增加用户到组织中。超级管理员将使用网站管理组员权限。每一个组织有一个developers组，所有的用户都会自动加入该组
The super admin is the only person who can add users to the org because it impacts the monthly bill. The super admin will use the website to manage membership. Every org has a developers team that all users are automatically added to.

The team admin is the person who manages team creation, team membership, and package access for teams. The team admin grants package access to teams, not individuals.

The developer will be able to access packages based on the teams they are on. Access is either read-write or read-only.

### npm folders

npm 放置了一些东西在你的电脑中，他处理一些事情。这份文档将告诉你它把它们放置在了哪里

#### NODE模块 - Node Modules

包根据前缀放置在了**node_modules**文件夹下
- 当本地安装时，意味着你可以这样` require("packagename")`使用它加载到你的主模块中
- 当全局安装时，会放置在系统根目录下，unix系统放置在`{prefix}/lib/node_modules.`，windows系统放置在`{prefix}/node_modules`
- 作用域包被下载时采用相同的处理方式，区别在于它们将组织在一起，并相对于node_modules目录，放在一个子文件夹中，使用作用域前缀`@scopename`作为文件名

#### 可执行文件 - Executables

当全局安装时，可执行文件将链接到指定位置：`{prefix}/bin` on Unix, or directly into `{prefix}` on Windows.

当本地安装时，可执行文件将会链接到当前工程的`./node_modules/.bin`目录下

#### 操作文档 - Man Pages

当全局安装时，操作文档将安装到指定位置：`{prefix}/share/man`

当本地安装时，不会安装操作文档

windows系统不会安装操作文档

#### 缓存 - Cache

缓存文件将npm-config配置存储到指定位置，默认存储在 `~/.npm` on Posix, or `~/npm-cache` on Windows.

#### 临时文件 - Temp Files

临时文件将根据tmp配置存储到指定位置，默认存储在`/tmp` on Unix and `c:\windows\temp` on Windows.

临时文件在程序每次运行时提供了一个唯一的目录，它们会在完成或退出删除

## 配置选项 - npm config

### 环境变量

`npm-config`的配置项可以可以通过`process.env`访问到，配置项使用`npm_config_`和`NPM_CONFIG_`前缀，如`npm_config_foo=bar`将会得到`foo`字段为`bar`值，如果某个字段未定义值，默认为`true`

### .npmrc 存放路径

有四处地方存在着`npmrc`配置文件
- 当前目录配置文件 - (/path/to/my/project/.npmrc)
- 当用用户的配置文件 - ($HOME/.npmrc)，也可以通过`configurable via CLI option --userconfig or environment variable $NPM_CONFIG_USERCONFIG`
- 全局配置文件 -   ($PREFIX/etc/npmrc)，也可以通过`configurable via CLI option --globalconfig or environment variable $NPM_CONFIG_GLOBALCONFIG`
- npm内置配置文件 -  (/path/to/npm/npmrc)

### 我的配置

用户级别配置
```bash
init-author-name = lisfan
init-author-email = goolisfan@gmail.com
init-author-url = https://www.npmjs.com/~lisfan
init-license = MIT
```

项目级别配置
```bash
registry = 新源地址
engine-strict = true
prefer-offline = true 
save-prefix = ~
package-lock = false
shrinkwrap = false
send-metrics = true
```

### 配置选项大全

#### 配置文件相关配置

- `init-module` - string : **~/.npm-init.js**
> 使用`npm init`命令初始化`package.json`文件的模板
> A module that will be loaded by the npm init command. See the documentation for the init-package-json module for more information, or npm-init.

- `init-author-name` - string : **''**
> `npm init`默认设置包作者名
> The value npm init should use by default for the package author's name.

- `init-author-email` - string : **''**
> `npm init`默认设置包作者邮箱
> The value npm init should use by default for the package author's email.

- `init-author-url` - string : **''**
> `npm init`默认设置包作者网址
> The value npm init should use by default for the package author's homepage.

- `init-license` - string : **ISC**
> `npm init`默认设置包的开源协议
> The value npm init should use by default for the package license.

- `init-version` - string : **1.0.0**
> `npm init`默认设置包的版本
> The value that npm init should use by default for the package version number, if not already set in package.json.

- `userconfig` - path : ** ~/.npmrc**
> 用户级别的配置文件存放地址
> The location of user-level configuration settings.

- `globalconfig` - path : **{prefix}/etc/npmrc**
> 全局配置文件路径
> The config file to read for global config options.

- `prefix` - path :  **npm-folders的配置**
> 指定全局`npm`依赖包的安装地址
> The location to install global items. If set on the command line, then it forces non-global commands to run in the specified folder.

- `tmp` - path - **/tmp**
> 临时目录，成功后会删除临时目录，失败记录都会保留
> Where to store temporary files and folders. All temp files are deleted on success, but left behind on failure for forensic purposes.

#### 信息输出展示相关的配置

- `user-agent` - string : **node/{process.version} {process.platform} {process.arch}**
> node请求时的用户代理字符串
> Sets a User-Agent to the request header

- `usage`  - boolean : **false**
> 是否输出简洁版的help文档（即使用了 -H参数）
> Set to show short usage output (like the -H output) instead of complete help when doing 

- `loglevel` - string : "silent（静默）", "error", "warn", "**notice**", "http", "timing", "info", "verbose", "silly"
> 日志报告级别。当发生失败时，所有的日志都会写进当前的目录`npm-debug.log`
> What level of logs to report. On failure, all logs are written to npm-debug.log in the current working directory.
> Any logs of a higher level than the setting are shown. The default is "notice".

- `logstream` - Stream : **process.stderr**
> npm日志使用的模块，默认使用在运行时使用`npmlog`模块

- `logs-max` - number : **10**
> 保留的日志文件最大数量
> The maximum number of log files to store.

- `long` - boolean : **false**
> 在`npm ls`和`npm search`中显示详细的扩展信息（简介、git地址、官网）
> Show extended information in npm ls and npm search.

- `progress` - boolean : **true**
> 一些耗时操作是否展示一个进度条

- `json` - boolean : **false**
> 某些命令是否自动以json格式展示数据，如`npm ls --json`
> Whether or not to output JSON data, rather than the normal output.

- `heading` - string : **npm**
> debugging日志输出前缀
> The string that starts all the debugging log output.

- `color` - boolean or "always" : **true**
> 终端是是否显示颜色
> If false, never shows colors. If "always" then always shows colors. If true, then only prints color codes for tty file descriptors.

- `depth` - number : **infinity**
> 使用`npm ls`, `npm cache ls`, and `npm outdated`时查看依赖目录的层级深度
> The depth to go when recursing directories for npm ls, npm cache ls, and npm outdated.
> For npm outdated, a setting of Infinity will be treated as 0 since that gives more useful information. To show the outdated status of all packages and dependents, use a large integer value, e.g., npm outdated --depth 9999

- `browser` - string : **OS X: "open", Windows: "start", Others: "xdg-open"**
> 使用`npm docs`命令时不同os调用底层命令打开浏览器的
> The browser that is called by the npm docs command to open websites.

- `editor` - string : **"vi" on Posix, or "notepad" on Windows.**
> 运行`npm edit`命令时，默认调用的编辑器

- `shell` - path : **SHELL environment variable, or "bash" on Posix, or "cmd" on Windows**
> The shell to run for the `npm explore` command.

- `timing` - boolean : **false**
> 记录错误日志时，记入时间信息
> If true, writes an **npm-debug** log to **_logs** and timing information to **_timing.json**, both in your cache. **_timing.json** is a newline delimited list of JSON objects. You can quickly view it with this json command line: **json -g < ~/.npm/_timing.json**

- `unicode` - boolean : **true**
> 若设置为true，则在npm中使用unicode字符，否则使用ascii字符 
> When set to true, npm uses unicode characters in the tree output. When false, it uses ascii characters to draw trees.

#### 脚本运行相关的配置

- `script-shell` - string : **null**
> 配置自定义的`shell`来运行`npm run`指令
> The shell to use for scripts run with the npm run command.

- `onload-script` - boolean : **false** , path
> 当npm加载完成的时候通过require加载的node模块，是一个文件路径。对编程起步教程很有帮助
> A node module to require() when npm loads. Useful for programmatic usage.

- `ignore-prepublish` - boolean : **false**
> 设置为true，在提交之前忽略`prepublish` 脚本的运行
> If true, npm will not run prepublish scripts.

- `ignore-scripts` - boolean : **false**
> 设置为true，无法执行script脚本
> If true, npm does not run scripts specified in package.json files.

- `scripts-prepend-node-path` -  boolean "auto" or **"warn-only"**
> (来源：网络翻译)运行脚本之前不需要用 node 可执行路径来运行 npm，可以通过添加 --scripts-prepend-node-path 选项来配置这个行为。
> If set to true, add the directory in which the current node executable resides to the PATH environment variable when running scripts, even if that means that npm will invoke a different node executable than the one which it is running.
> If set to false, never modify PATH with that.
> If set to "warn-only", never modify PATH but print a warning if npm thinks that you may want to run it with true, e.g. because the node executable in the PATH is not the one npm was invoked with.
> If set to auto, only add that directory to the PATH environment variable if the node executable with which npm was invoked and the one that is found first on the PATH are different.

#### 包管理相关的配置

- `registry` - string : **https://registry.npmjs.org/**
> npm源地址，默认指向官方源

- `access` -  string : **restricted**(scoped package)  | **public**(unscoped package)
> 作用域包是否公共可见，默认值进行限制，不可见。非局部包默认公共可见
> 注：所以及需要将作用域包发布时，需要将参数改成代我
> When publishing scoped packages, the access level defaults to restricted. If you want your scoped package to be publicly viewable (and installable) set --access=public. The only valid values for access are public and restricted. Unscoped packages always have an access level of public.

- `scope` - string : **''**
> Associate an operation with a scope for a scoped registry. Useful when logging in to a private registry for the first time: npm login --scope=@organization --registry=registry.organization.com, which will cause @organization to be mapped to the registry for future installation of packages specified according to the pattern @organization/package.

- `dry-run` - boolean : **false**
> 表示你不想让`npm`做出任何变化，且它只会提供一个运行结果报告，看看npm做了什么
> Indicates that you don't want npm to make any changes and that it should only report what it would have done. This can be passed into any of the commands that modify your local installation, eg, `install`, `update`, `dedupe`, `uninstall`. This is NOT currently honored by network related commands, eg dist-tags, owner, publish, etc.

> `npm version`提升包版本时的版本前缀 
> If set, alters the prefix used when tagging a new version when performing a version increment using **npm-version**. To remove the prefix altogether, set it to the empty string: "".
> Because other tools may rely on the convention that npm version tags look like **v1.0.0,** only use this property if it is absolutely necessary. In particular, use care when overriding this setting for public packages.

- `allow-same-version` - boolean : **false**
> 当使用`npm version <version>`设置版本号时设置与当前包相同的版本时，是否阻止抛出错误
> Prevents throwing an error when npm version is used to set the new version to the same value as the current version.

- `message` - string : **%s**
> `npm version`使用的提交信息，任何"%s"的信息将会被版本数替换
> Commit message which is used by npm version when creating version commit.
> Any "%s" in the message will be replaced with the version number.

- `tag-version-prefix` - string : **v**

- `legacy-bundling` - boolean : **false**
> 像npm1.4之前那样安装包，因此此时对于安装的包是不会去重的
> Causes npm to install the package such that versions of npm prior to 1.4, such as the one included with node 0.8, can install the package. This eliminates all automatic deduping. If used with global-style this option will be preferred.

- `global-style` - boolean : **false**
>  是否使用全局的**node_modules**架构来安装本地的模块。此时，只有直接的依赖会在**node_modules**中显示，其他的模块都是在依赖的模块的**node_modules**文件夹。此时会产生很多重复模块
>  Causes npm to install the package into your local node_modules folder with the same layout it uses with the global node_modules folder. Only your direct dependencies will show in node_modules and everything they depend on will be flattened in their node_modules folders. This obviously will eliminate some deduping. If used with legacy-bundling, legacy-bundling will be preferred.

#### 依赖包安装相关的配置

- `engine-strict` - boolean : **false**
> 设置为true时，会拒绝安装不符合当前node版本的npm包
> If set to true, then npm will stubbornly refuse to install (or even consider installing) any package that claims to not be compatible with the current Node.js version.

- `node-version` - string | false : **process.version**
> 会用于检测当前npm包的engines是否匹配当前node版本
> The node version to use when checking a package's engines map.

- `shrinkwrap`- boolean : **true**
> `package-lock`配置项的别名
> 设置为false，会忽略`npm-shrinkwrap.json`
> If set to false, then ignore `npm-shrinkwrap.json` files when installing. This will also prevent writing npm-shrinkwrap.json if save is true.

- `package-lock` - boolean : **true**
> 是`shrinkwrap`配置项的别名
> 设置为false，会忽略`package-lock.json`
> If set to false, then ignore package-lock.json files when installing. This will also prevent writing package-lock.json if save is true.

- `rebuild-bundle` - boolean : **true**
> 安装特定的包以后，重新构建打包的那些依赖（npm3的新功能）
> Rebuild bundled dependencies after installation.

- `rollback` - boolean : **true**
> 移除那些没有安装成功的包
> Remove failed installs.

- `link` - boolean : **false**
> 设置为true时，使用`npm install`下载包时，若在全局包目录中已存在，则会将全局包目录中的指定包以软链接映射的方式安装到当前项目，若不存在则会将源安装到全局包目录中再软链接过来
> 会发生link的条件：这个包没有全局安装 + 全局安装的包和局部安装的包的版本一致
> If true, then local installs will link if there is a suitable globally installed package.
> Note that this means that local installs can cause things to be installed into the global space at the same time. The link is only done if one of the two conditions are met:
> The package is not already installed globally, or
> the globally installed version is identical to the version that is being installed locally.

- `bin-links` - boolean : **true**
> 阻止npm对任何可能包含的二进制包创建符号链接symlinks
> Tells npm to create symlinks (or .cmd shims on Windows) for package executables.
> Set to false to have it not do this. This can be used to work around the fact that some file > systems don't support symlinks, even on ostensibly Unix systems.

- `save-prefix` - string : **^**，~
> npm包安装后，记录到`package.json`中如何进行版本管理
> Configure how versions of packages installed to a package.json file via --save or --save-dev get prefixed.

- `tag` - string : **latest**
> `npm install`时如果未指定依赖包的版本号，则默认使用此给定的参数
> If you ask npm to install a package and don't tell it a specific version, then it will install the specified tag.
> Also the tag that is added to the package@version specified by the `npm tag` command, if no explicit tag is given.

- `also` - string : **null**
> 设置为`dev`或`development`时，运行如下命令 `npm shrinkwrap`, `npm outdated`, or `npm update`，相当于添加了 `--dev` 参数
> When "dev" or "development" and running local npm shrinkwrap, npm outdated, or npm update, is an alias for --dev.

- `only` - string : **null**，dev，development，prod，production
> 当设置为`dev`时，使用`npm install`命令只安装`devDependencies`字段的开发依赖包，使用`npm ls，npm outdated, or npm update`相当于自动附加了`--dev` 参数
> 当设置为`prod`时，使用`npm install`命令只安装`dependencies`字段的生产依赖包，使用`npm ls，npm outdated, or npm update`相当于自动附加了`--prod` 参数
> When `dev` or `development` and running local npm install without any arguments, only devDependencies (and their dependencies) are installed.
> When `dev` or `development` and running local npm ls, npm outdated, or npm update, is an alias for `--dev`.
> When `prod` or `production` and running local npm install without any arguments, only non-devDependencies (and their dependencies) are installed. 
> When `prod` or `production` and running local npm ls, npm outdated, or npm update, is an alias for `--production`.

- `global` - boolean : **false**
> 启用全局`global`安装模式，即不再需要使用`-g`这样方式
> Operates in "global" mode, so that packages are installed into the prefix folder instead of the current working directory. See npm-folders for more on the differences in behavior.

- `dev` - boolean : **false**
> 只下载devDependencies字段的依赖
> Install dev-dependencies along with packages.

- `production` - boolean  : **false**
> `npm install`时不会安装`devDependencies`字段的依赖包
> 同时设置`NODE_ENV="production"`
> Set to true to run in "production" mode.
> `devDependencies` are not installed at the topmost level when running local npm install without any arguments.
> Set the `NODE_ENV="production"` for lifecycle scripts.

- `optional` - boolean : **true**
> 是否安装可选依赖包
> 尝试安装`optionalDependencies`字段的依赖包，如果安装失败，并不会进行提示
> Attempt to install packages in the `optionalDependencies` object. Note that if these packages fail to install, the overall installation process is not aborted.

- `save` - boolean : **true**
> 当`package.json`存在时，使用`npm install`或 `npm rm` 会对`dependencies`字段进行增减
> Save installed packages to a package.json file as dependencies.
> When used with the npm rm command, it removes it from the dependencies object.
Only works if there is already a package.json file present.

- `save-bundle` - boolean : **false**
> 当`package.json`存在时，使用`npm install`或 `npm rm` 会对`bundleDependencies`字段进行增减

- `save-optional` - boolean : **false**
> 当`package.json`存在时，使用`npm install`或 `npm rm` 会对`optionalDependencies`字段进行增减

- `save-prod` 作用同`save-bundle`，对`dependencies`字段进行增减，但会与`svae-dev`同时设置产生冲突
- `save-dev` 作用同`save-bundle`，对`dependencies`字段进行增减，但会与`svae-prod`同时设置产生冲突

- `save-exact` - boolean : **false**
- 当使用`--save`、`--save-dev`、`--save-optional`依赖保存到`package.json`时，使用一个确定的版本，而不是使用默认的版本范围符号

- `fetch-retries` - number : **2**
> 设置依赖包拉取失败时的重试次数
> The "retries" config for the retry module to use when fetching packages from the registry.

- `fetch-retry-factor` - number : **10**
> 设置依赖包拉取失败时的重试次数
> The "factor" config for the retry module to use when fetching packages.

- `fetch-retry-mintimeout` - number : **10000**
> 设置依赖包的拉取的最小超时时间

- `fetch-retry-maxtimeout` - number : **60000**
> 设置依赖包的拉取的最大超时时间

#### 搜索相关配置

- `description` - boolean : **true**
> 使用`npm search`时显示显示描述展示
> Show the description in npm search

- `parseable` - boolean : **false**
> 针对`npm search`搜索结果输出使用一个`tab`缩进的表格格式

- `searchexclude` - string : **''**
> Space-separated options that limit the results from search

- `searchopts` - string : **''**
> Space-separated options that are always passed to search.

- `searchlimit` - number : **20**
> 搜索限制条数
> Number of items to limit search results to. Will not apply at all to legacy searches.

- `searchstaleness` - number : **900**
> 搜索缓存时间
> The age of the cache, in seconds, before another registry request is made if using legacy search endpoint.

#### 缓存控制相关配置

- `cache` - string : **Windows: %AppData%\npm-cache, Posix: ~/.npm**
> npm包的本地缓存目录
> The location of npm's cache directory. See npm-cache

- `cache-lock-retries` - number : **10**
> 获取缓存中某一个加锁文件最大的尝试次数
> Number of times to retry to acquire a lock on cache folder lockfiles.

- `cache-lock-stale` - number : **60000**
> 过了多少毫秒后才需要判断该缓存文件是否过期
> The number of ms before cache folder lockfiles are considered stale.

- `cache-lock-wait` - number : **10000**
> 缓存文件过期需要等待多少毫秒
> Number of ms to wait for cache lock files to expire.

- `cache-max` - - number : **Infinity**  (废弃，使用 --prefer-ofline)
> 在重新检查注册（registry ）之前，在注册表中保存缓存项目的最大时间（秒）。注意，并没有清除缓存，除非用了npm cache clean 

- `cache-min` - number : **10** 分钟 (废弃，使用 --prefer-ofline)
> 某人见解：运行`npm install`的时候，只会检查**node_modules**目录，而不会检查**~/.npm**目录。也就是说，如果一个模块在**~/.npm**下有压缩包，但是没有安装在**node_modules**目录中，`npm`依然会从远程仓库下载一次新的压缩包。 这种行为固然可以保证总是取得最新的代码，但有时并不是我们想要的。最大的问题是，它会极大地影响安装速度。即使某个模块的压缩包就在缓存目录中，也要去远程仓库下载，这怎么可能不慢呢？ 另外，有些场合没有网络或者网络慢（比如飞机上），但是你想安装的模块，明明就在缓存目录之中，这时也无法安装。为了解决这些问题，`npm` 提供了一个`--cache-min`参数，指定一个时间（单位为分钟），只有超过这个时间的模块，才会从 registry 下载，用于从缓存目录安装模块，而判断是否时间就是通过我们第一点讲的.cache.json的_lastModified来判断的。

- `prefer-offline` - boolean : **false**
> If true, staleness checks for cached data will be bypassed, but missing data will be requested from the server. To force full offline mode, use --offline.
> This option is effectively equivalent to --cache-min=9999999.

- `prefer-online` - boolean : **false**
> If true, staleness checks for cached data will be forced, making the CLI look for updates immediately even for fresh package data.

- `offline` - boolean : **false**
> 强制断网模式，npm包下载时不发起网络请求(作不用是这个)
> Force offline mode: no network requests will be done during install. To allow the CLI to fill in missing cache data, see --prefer-offline.

#### 其他

- `umask` - Octal Number : **022**
> 设置文件和目录的权限掩码
> Folders and executables are given a mode which is 0777 masked against this value. Other files are given a mode which is 0666 masked against this value. Thus, the defaults are 0755 and 0644 respectively.

- `version` - boolean : **false**
> If true, output the npm version and exit successfully.
> Only relevant when specified explicitly on the command line.

- `force` - boolean : **false**
> 让一些命令更加暴力
> Makes various commands more forceful.
- 运行中脚本失败不会阻碍后续 - lifecycle script failure does not block progress.
- 彻底摧毁前一个发布的版本 - publishing clobbers previously published versions.
- 请求源时忽略缓存 - skips cache when requesting from the registry.
- prevents checks against clobbering non-npm files.

- `git` - boolean
> default: **git**
> 如果安装了git，但没有将git加到`PATH`中，无法直接以命令行方式使用，可以以`npm git` 这样的方式调用

- `git-tag-version` - boolean
> default: **true**
> 使用`npm version`命令时，自动为打上`git tag`

- `sign-git-tag` - boolean
> default:**false**
> 使用`npm version`时将会为`git`增加一个`tag`

#### 安全认证相关配置

- `always-auth` - boolean : **false**
> 每次get方式访问源时，强制npm进行认证
> Force npm to always require authentication when accessing the registry, even for GET requests.

- `auth-type` - string : **legacy**，sso，saml，oauth
> 注册和登陆时的验证策略
> What authentication strategy to use with adduser/login.

- `ca` - ca证书
- `cafile` - 包含ca证书的文件路径 
- `cert`
- `key` - string 
- `proxy`
- `sso-poll-frequency`
- `sso-type`
- `strict-ssl`
- `unsafe-perm` - true

#### 以下是不明作用的配置

- `send-metrics` - boolean : **false**
> If true, success/failure metrics will be reported to the registry stored in metrics-registry. These requests contain the number of successful and failing runs of the npm CLI and the time period overwhich those counts were gathered. No identifying information is included in these requests.

- `metrics-registry` - path : **https://registry.npmjs.org/**
> The registry you want to send cli metrics to if send-metrics is true.

- `local-address` - string : **undefined**
> 请求源时，通过该ip进行请求
> The IP address of the local interface to use when making connections to the npm registry. Must be IPv4 in versions of Node prior to 0.12.

- `maxsockets` - number : **50**
> 通过`http Agent`时，每个源(包含协议，域名，端口)最大的socket请求链接数
> The maximum number of connections to use per origin (protocol/host/port combination). Passed to the http Agent used to make the request.

- `https-proxy` - url : **null**
> https代理
> A proxy to use for outgoing https requests. If the HTTPS_PROXY or https_proxy or HTTP_PROXY or http_proxy environment variables are set, proxy settings will be honored by the underlying request library.

- `if-present` - boolean : **false**
> 如果设置为true，那么package.json的script部分(通过npm run运行的部分)不存在该script的时候不会抛出错误代码。当这个script可选的时候非常有用
> If true, npm will not exit with an error code when run-script is invoked for a script that isn't defined in the scripts section of package.json. This option can be used when it's desirable to optionally run a script when it's present and fail if the script fails. This is useful, for example, when running scripts that may only apply for some builds in an otherwise generic CI setup.

- `group` - number | string : **GID of the current process**
> 当以root用户来执行包中的script文件的时候使用什么组，默认为用户的GID。http://blog.chinaunix.net/uid-30126070-id-5073564.html
> The group to use when running package scripts in global mode as the root user.

- `user` - string : **nobody**
- 当以root用户运行package.json中的script的时候设置的UID（默认为"nobody"）
> The UID to set to when running package scripts as root.

## CLI 命令

### 常用命令

对于多个npm run命令使用&&来连接，而如cross-env来说，它只是设置NODE_ENV，其和其他命令之间不需要&&来分割

### 命令大全

```
# 使用npm命令
npm <command> [<args>]

# 查看npm的含有的命令及简要文档
npm -l

# 查看具体npm命令的快速帮助文档（很简单）
npm <command> -h

# 查看具体npm命令的帮助文档（最详细）
npm help <command>

# 查看npm描述
npm help npm
```

`<command>` 中的值可以是以下任意一项
- 依赖包列表展示：`list`，别名：`ls`、`ll`、`la`
- 包链接建立：`link`，别名：`ln`
- 依赖安装：`install`，别名：`i`、`add`、`isntall`
- 依赖安装并运行test钩子脚本：`install-test`，别名：`it`
- 依赖卸载：`uninstall`，别名：`un`、`unlink`、`remove`、`rm`、`r`
- 依赖更新：`update`，别名：`up`、`upgrade`、 `udpate`
- 依赖过期检查：`outdated`
- 依赖排重：`dedupe`，别名：`ddp`、`find-dupes`
- 依赖裁剪：`prune`
- 依赖重建：`rebuild`，别名：`rb`
- 依赖锁定：`shrinkwrap`
- 包搜索：`search`，别名：`s`、`se`、`find`
- 包信息查看：`view`，别名：`v`、`info` 、`show`
- 包主页查看：`docs`，别名：`home`
- 包问题反馈：`bugs`，别名：`issues`
- 包仓库查看：`repo`
- 包标星：`star`
- 查看某作者所有标星包：`stars`
- 当前包脚本运行：`run-script`， 别名：`run`、`rum`
- 当前包start钩子运行：`start`
- 当前包stop钩子运行：`stop`
- 当前包restart钩子运行：`restart`
- 当前包test钩子运行：`test`，别名：`tst`、`t`
- 当前包访问权限管理：`access` 
- 当前包初始化：`init`
- 当前包版本更新：`version`
- 为包标记tag：`dist-tag`，别名：`dist-tags`
- 当前包发布：`publish` 
- 当前包打包：`pack`
- 包版本撤回：`unpublish` 
- 包版本废弃：`deprecated` 
- 包的拥有者权限管理：`owner`，别名：`author`
- 源当前用户：`whoami` 
- 源用户注册/登陆：`adduser` ，别名：`login`、`add-user`
- 源用户登出：`logout`
- 源个人资料配置 - `profile`
- 源token管理 - `token`
- 源用户组管理：`team`
- 缓存管理：`cache`
- npm配置管理：`config`，别名：`c`，快捷方式使用：`get`与`set`设置
- npm进行自检：`doctor`
- bin文件目录：`bin`
- npm-config配置目录: `prefix`
- npm的依赖包安装目录: `root`
- 已安装依赖包浏览: `explore`
- 已安装依赖包编辑: `edit`
- 帮助文档查看：`help`
- 帮助文档查找：`help-search`
- 提供bash的命令自动补全功能：`completion`
- 测试源网络是否连通：`ping`

### 参数大全

- `-v | --version` - 查看 npm 版本
- `-? | -h | -H | --help | --usage` - 查看帮助
- `-s | --slient | --loglevel silent` - 日志级别：静默
- `-q | --quiet | --loglevel warn` - 日志级别：警告
- `-d | --loglevel info` - 日志级别：较信息信信息
- `-dd | --verbose | --loglevel verbose` - 日志级别：冗长的信息
- `-ddd | --loglevel silly` - 日志级别：更冗长的信息
- `-g | --global` - 针对全局
- `-C | --prefix` - 使用指定的npm配置
- `-l | --long` - 显示长信息
- `-m | --message`
- `-p | --parseable` - 可解析数据
- `-reg | --registry` - 设置npm包的下载服务地址
- `-f | --force` - 强制执行
- `-desc | --description` - 显示详细信息
- `-json` - 以json格式显示
- `-y | --yes`
- `-n | --yes false` - yes false

命令行参数支持联想

```
npm ls --par
# same as:
npm ls --parseable
```

命令行参数支持多个同时使用

```
npm ls -gpld
# same as:
npm ls --global --parseable --long --loglevel info
```

#### 依赖包列表展示 - npm ls

```bash
# 列出已安装的依赖民
npm ls [[<@scope>/]<package>[@<version>]...] [<args>]
```

支持的args参数有
- `--lone` - 显示扩展信息
- `--json` - 以json格式显示信息
- `--parseable` - 显示包的安装路径
- `--global` - 显示全局下的包
- `--dev` - 只显示开发环境的包
- `--prod` - 显示生产环境的包
- `--only=dev` -  安装package.json中的devDependencies依赖
- `--only=prod` -  安装package.json中的dependencies依赖
- `--link` - 显示链接的包
- `--depth` - 显示依赖的层级

```bash
# 查看该项目安装的依赖包
npm ls 

# 查看全局安装的依赖包
npm ls -g

# 查看该项目安装的依赖包，只展示顶级依赖
npm ls --depth=0

# 查找该项目安装的依赖包的具体包的信息(比如我想查找该项目下依赖到的所有lodash版本)
npm ls <package-name>
```

#### 包链接建立 - npm link

```bash
# 链接一个包目录
npm link (in package dir)
npm link [[<@scope>/]<package>[@<version>]]

# 1. 将当前包链接到全局
npm link
# 2. 在用到的项目中链接包
npm link <package>
```

包建立链接需要二步
1. 在当前包中运行 npm link，将其链接到全局node_modules中
2. 使用npm link `<package>` 链接到你的项目中，注意package是在package.json中指定的名称，并不是link时的文件名

#### 依赖安装 - npm install

```bash
# 安装一个包
npm install (with no args, in package dir) # 根据package.json文件的配置安装依赖
```

支持的args参数有
- `--global | -g` - 会将所有的模块进行全局安装
- `--prod| --production`  - (deprecated)安装package.json中的dependencies依赖
- `--only=dev` -  安装package.json中的devDependencies依赖
- `--only=prod` -  安装package.json中的dependencies依赖

```bash
# 格式
npm install [<@scope>/]<package>... # 安装指定npm-config配置tag指向的包版本，默认tag指向`latest`
npm install [<@scope>/]<package>[@<tag>]... # 安装指定tag版本的包
npm install [<@scope>/]<package>[@<version>]... # 安装指定version版本的包
npm install [<@scope>/]<package>[@<version range>]... # 安装指定版本范围的包

# example
npm install lodash
npm install lodash@beta
npm install lodash@2.0.0
npm install lodash@">=2.0.0 <3.0.0" # 必须要有引号
```

```bash
# 从git服务提供都安装包，使用git克隆。
npm install <git-host>:<git-user>/<repo-name> # git-host形式
npm install <githubname>/<githubrepo>[#<commit-ish>]
npm install github:<githubname>/<githubrepo>[#<commit-ish>]
npm install gist:[<githubname>/]<gistID>[#<commit-ish>|#semver:<semver>]
npm install bitbucket:<bitbucketname>/<bitbucketrepo>[#<commit-ish>]
npm install gitlab:<gitlabname>/<gitlabrepo>[#<commit-ish>]

# example 
npm install git+ssh://git@github.com:lisfan/logger.git
```

一个完整理的git地址应该符合这种格式：

```
<protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish> | #semver:<semver>]
```

-  `<protocol>` 是`git`、`git+ssh`、`git+http`、`git+https`、`git+file`协议中的一个
- 如果提供了`#<commit-ish>`，它将会克隆使用一个确切的提交分支
- 如果提供了`#semver:<semver>`，semver可以是任何有效的语义版本范围或者确切的版本
- 如果未提供`#<commit-ish>`，也未提供`#semver:<semver>`，则默认使用`master`
- If the repository makes use of submodules, those submodules will be cloned as well.


```bash
# 拉取tar包链接，然后安装它。链接必须是"http://" or "https://"开头
npm install <tarball url> 

# example 
npm install https://registry.npmjs.org/@~lisfan/validation/-/validation-1.0.1.tgz
```

```bash
# 安装本地 tar 包格式的文件，文件扩展名必须使用`.tar`、`tar.gz`、`tgz`
npm install <tarball file>

# example
npm install ./validation-1.0.1.tgz
```

```bash
# 将指定的文件夹（该文件夹符合包的格式）作以软链接方式安装到当前项目，该文件夹的依赖会在链接前
npm install <folder> 

# example
npm install ./validation
```

除了直接的`npm install`命令外，其他格式均支持以下args参数
- `--registry` - 设置源目标
- `--global | -g` - 全局安装
- `--save-prod | -P`  - 安装为dependencies依赖
- `--save-dev | -D`  - 安装为devDependencies依赖
- `--save-optional | -O`  - 安装为optionalDependencies依赖
- `--save-bundle | -B`  - 安装为bundleDependencies依赖
- `--save-exact | -E` - 安装为依赖时，使用一个确切的版本号
- `--no-save` - 阻止保存到dependences里
- `--dry-run` - 该参数将在你执行`npm install`命令时，它不会产生真正的下载安装行为，但会提供一个有用的报告（即模拟了下载安装行为）
- `--force | -f` - 即使有一份本地拷贝存在磁盘上，也强制npm拉取远程资源
- `--ignore-scripts` - 阻止执行package.json中定义的scripts
- `--link` - 将依赖包装到全局并以软链的方式引用
- `--no-bin-links` - 阻止npm为包中包含的二进制文件创建软链
The  argument will prevent npm from creating symlinks for any binaries the package might contain.
- `--no-optional` - 阻止安装optionDependencies依赖
- `--no-shrinkwrap` - 忽略生成或更新`package-log`或`npm-shrinkwrap`文件，但会更新`package.json`
- `--no-package-lock` - 阻止npm创建`package-lock.json`
- `--nodedir=/path/to/node/source` - argument will allow npm to find the node source code so that npm can compile native modules.

#### 依赖安装并运行测试 - npm install-test

```bash
# 安装一个包，并运行test脚本
npm install-test [[<@scope>/]<package>[@<version>]...] [<args>]
```

这个命令运行`npm install`后立即跟着运行`npm test`，他可以使用跟`npm install`相同的参数


#### 依赖卸载 - npm uninstall

```bash
# 卸载一个包，完全移除安装时的所有东西
npm uninstall [[<@scope>/]<package>[@<version>]...] [<args>]

# 卸载当前项目依赖
npm uninstall lodash

# 卸载全局依赖
npm uninstall -g lodash
```

支持的args参数有
- `--global | -g` - 移除全局依赖
- `--save| -S`  -  包会从你的 dependencies 依赖中移除
- `--save-dev| -D`  - 包会从你的 devDependencies 依赖中移除
- `--save-optional| -O`  - 包会从你的 optionalDependencies 依赖中移除
- `--no-save` - 包不会从你**package.json**文件中移除


#### 依赖更新 - npm update

```bash
# 更新一个包
npm update [[<@scope>/]<package>[@<version>]...] [<args>]

# 更新所有依赖
npm update

# 更新指定依赖
npm update lodash
```

这个命令将更新依赖的版本，它会遵循**package.json**中指定的**语义版本**，**即不会跨版本升级**，若需要替代新版本，请使用`npm install`代替。它也会更新丢失的依赖

支持的args参数有
- `--global | -g` - 移除全局依赖
- `--save| -S`  -  包会从你的 dependencies 依赖中移除

#### 依赖过期检查 - npm outdated

```bash
# 检查过期的依赖包
# 检查依赖是否有新版
npm outdated [[<@scope>/]<package>[@<version>]...] [<args>]

# 检查所有依赖
npm outdated

# 检查指定依赖
npm outdated lodash
```

这个命令将检查从源上检查安装的包是否已经过期
命令会在检查后输出一个报告，展示了几个字段
- package - 依赖包的名称
- current - 显示当前版本
- wanted - 显示满足**package.json**指定语义版本的最新版本
- latest - 显示源上latest标签的版本
- location - 该依赖安装的位置

支持的args参数有
- `--lone` - 显示扩展信息
- `--json` - 以json格式显示信息
- `--parseable` - 显示包的安装路径
- `--global` - 显示全局下的包
- `--depth` - 显示依赖的层级

#### 依赖排重 - npm dedupe

```bash
# 减少重复
# 重复依赖进行排重，同时整理依赖树的结构，它会遵循package.josn的语义版本
npm dedupe [<args>]
```

支持的args参数有
- `--global | -g` - 整理全局模块

#### 依赖裁剪 - npm prune

```bash
# 移除无用的包
# 该命令会检查package.json文年
npm prune [[<@scope>/]<package>[@<version>]...] [<args>]

# 全部检查
npm prune 

# 检查具体的包
npm prune lodash
```

支持的args参数有
- `--production` - 检查`devDependencies`字段

#### 依赖重建 - npm rebuild

```bash
# 重建一个包
# 这个命令运行npm build 命令在匹配的文件夹，当你安装了一个新版本的node后
npm rebuild [[<@scope>/]<package>[@<version>]...]

# 重建所有
npm rebuild

# 重建指定依赖
npm rebuild lodash
```

#### 依赖锁定 - npm shrinkwrap

```bash
# 发布包时锁定最低依赖版本
# 这个命令会更改packag-lock.json为一个可发布的npm-shrinkwrap.json，或者重新创建一个新的文件
npm shrinkwrap
```

#### 包搜索 - npm search

```bash
# 搜索包
# 在源上匹配的包
npm search [<args>] [-l|--long] [--json] [--parseable] [--no-description] [search terms ...]
```

支持的args参数有
- `--description` - Used as --no-description, disables search matching in package descriptions and suppresses display of that field in results.
- `--lone` - 显示扩展信息
- `--json` - 以json格式显示信息
- `--parseable` - 显示包的安装路径
- `--searchopts`
- `--searchexclude`
- `--searchstaleness`
- `--registry` - 设置源目标


#### 包信息查看 - npm view

```bash
# 显示源上的包信息
npm view [<@scope>/]<package>[@version] [<field>[.<subfield>]...]

# 查看包信息，默认查看latest标签的包
npm view lodash

# 查看指定字段
npm view lodash version

# 查看指定字段的子字段
npm view lodash dist-tags.latest

# 查看数组对象格式对应序位的字段(下面这种测试不支持，无奈)
npm view lodash contributors[0].email

# 查看数组对象格式对应的字段
npm view lodash contributors.email

# 同时查看两个字段
npm view lodash version author
```

支持的args参数有
- `--json` -  以json格式显示信息

#### 包主页查看 - npm docs
```bash
# 查看包主页
npm docs [[<@scope>/]<package>] [<args>]

# 查看指定包的主页
npm docs @~lisfan/validation

# 查看当前包主页
npm docs
```

支持的args参数有
- `--browser` -  设置浏览器
- `--registry` - 设置源目标

#### 包问题反馈 - npm bugs

```bash
# 查看包问题列表
npm bugs [[<@scope>/]<package>] [<args>]

# 查看指定依赖的issues
npm bugs @~lisfan/validation

# 查看当前包issues
npm bugs
```

支持的args参数有
- `--browser` -  设置浏览器
- `--registry` - 设置源目标

#### 包仓库查看 - npm repo

```bash
# 查看包仓库
npm repo [[<@scope>/]<package>] [<args>]
```

支持的args参数有
- `--browser` -  设置浏览器

#### 包标星 - npm star
```bash
# 标记你喜欢的包 
npm star [<@scope>/]<package>...
# 取消标记你喜欢的包 
npm unstar [<@scope>/]<package>...
```

#### 查看某用户所有标星包 - npm stars
```bash
# 查看某用户标记的所有喜欢的包
npm stars [<user>]

# 查看自已
npm stars 

# 查看别人
npm stars timmywil 
```

#### 当前包脚本运行 - npm run

```bash
# 运行任意的包脚本
# 这会运行一个来自package.json的scripts对象的任意的脚本命令。如果未指令脚本命令，则它会显示所有可用的脚本列表
npm run-script <script-name> [--silent] [<args>]
```

支持的args参数有
- `--silent` -  防止npm ERR!时的错误显示输出

#### 当前包 start 脚本运行 - npm start
```bash
# 运行包
npm start [<args>]
```

#### 当前包 stop 脚本运行 - npm stop
```bash
# 停止运行包
npm stop [<args>]
```

#### 当前包 test 脚本运行 - npm test
```bash
# 测试包
npm test [<args>]
```

#### 当前包 restart 脚本运行 - npm restart
```bash
# 重启包
npm restart [<args>]
```

#### 当前包访问权限管理 - npm access

```bash
# 设置包的发布权限级别
# 当未指定package时，则所有的命令都作用于当前包
npm access [<args>]

# 设置为公共，只可为作用域包设置权限
npm access public [<package>]

# 设置为限制
npm access restricted [<package>]

# 授权用户组的读写权限
npm access grant <read-only|read-write> <scope:team> [<package>]

# 撤消用户组的包
npm access revoke <scope:team> [<package>]

# 展示用户或用户组管理的所有包
npm access ls-packages [<user>|<scope>|<scope:team>]

# 展示包的贡献者的权限
npm access ls-collaborators [<package> [<user>]]
npm access edit [<package>]
```

支持的args参数有
- `--registry` - 设置源目标

#### 当前包初始化 - npm init

```bash
# 交互式创建package.json文件
npm version [<args>]
```

这会问你一系列的问题，回答之后会为您写入一个**package.json**文件
如果你已经存在**package.json**文件，他先读取文件作为默认值选项

支持的args参数有
- `--scope` -  设置作用域
- `--force | -f` -  不会以交互方式提问，而是输出默认配置
- `--yes | -y` -  不会以交互方式提问，而是输出默认配置

#### 当前包版本更新 - npm version

```bash
# 发布新版本
npm version [<newversion> | major | minor | patch | premajor | preminor | prepatch | prerelease | from-git]

# 自定义版本，版本号应该是一个有效的语义版本
npm version 2.0.0

# 递增主版本号，假设当前为2.0.0
npm version major # => 3.0.0

# 递增修订版本号，假设当前为3.0.0
npm version major # => 3.0.1

# 递增前置发布版本号，假设当前为3.0.1-rc.0
npm version prerelease # => 3.0.1-rc.1

# 递增前置主布本号，假设当前为3.0.1-rc.0
npm version premajor # => 4.0.1-rc.1
```

支持的args参数有
- `--allow-same-version` -  是否允许定义相同的版本
- `--git-tag-version` -  Commit and tag the version change.
- `--commit-hooks` -  Run git commit hooks when committing the version change.
- `--sign-git-tag` -  Pass the -s flag to git to sign the tag.，Note that you must have a default GPG key set up in your git config for this to work properly.

#### 为包标记tag - npm dist-tag

```bash
# 更改包的输出标签
# 为某个包的某个版本设置tag标签
npm dist-tag add <package>@<version> [<tag>]

# 移除某包的tag标签
npm dist-tag rm <package> <tag>

# 显示包的所有tag标签
npm dist-tag ls [<package>]

# 显示当前包的所有tag标签
npm dist-tag ls
```

#### 当前包发布 - npm publish

```bash
# 当指定的源上已存在与该包相同的名称和版本时，会发布失败
npm publish [<tarball>|<folder>] [<args>]

# 发布当前包
npm publish
```

目录中的所有文件都会被提交，除了本地的`.gitignore`和`.npmignore`里忽略的项
且确保你相提交的npm包还不存在于npm网站上
.npmignore 使用和.gitignore相同的规则

支持的args参数有
	- `--registry` - 设置源目标

#### 当前包发布撤回 - npm unpublish

```bash
# 撤销已发布的包
npm unpublish [<@scope>/]<package>[@<version>]

# 撤销指定包
npm unpublish @~lisfan/logger
```

这是一个经过深思熟虑的操作行为用来移除当前的版本，即使这个版本还被其他人依赖

假如你想鼓励用户升级版本，考虑使用`deprecate`命令代替

撤销操作只允许发布后的24小时内。如果你想撤销一个以前的发布版本，请联系 `support@npmjs.com`

#### 当前包打包 - npm pack

```bash
# 从一个包中创建一个tar压缩包
# 打包
npm pack [[<@scope>/]<package>[@<version>]]

# 打包当前包
npm pack

# 打包指定包
npm pack jquery lodash
```
对于任何可下载方式的包，这个命令会拉取到缓存，之后拷贝以`<name>-<version>.tgz`命名的压缩包到你的工作目录

#### 指定包废弃 - npm deprecate

```bash
# 废弃一个包版本
# 给尝试下载该版本的开发者提供警告信息
npm deprecate [<@scope>/]<package>[@<version>] <message>
```

#### 包的拥有者权限管理 - npm owner

```bash
# 管理包的拥有者
# 增加一个新的用户作为包的维护者，这个用户可以更改元数据、发布新版本以及增加其他用户
npm owner add <user> [[<@scope>/]<package>]

# 从包的用户者列表者移除一个用户，它会立即撤销他们的权限一个新的用户作为包的维护者，这个用户可以更改元数据、发布新版本以及增加其他用户
npm owner rm <user> [[<@scope>/]<package>]

# 显示所有有权限更改和发布新版本的用户
npm owner ls [[<@scope>/]<package>]
```


#### 源当前用户 - npm whoami

```bash
# 展示源用户名
# 打印输出当前源的登陆用户
npm whoami [<args>]
```

支持的args参数有
	- `--registry` - 设置源目标

#### 源用户注册/登陆 - npm adduser

```bash
# 增加一个源用户
# 在指定的源上创建或验证一个用户，并保存用户凭证书到.npmrc文件（userconfig级别），未指定源时，使用默认源
npm adduser [<args>]
```

支持的args参数有
	- `--registry` - 设置源目标
	- `--scope` - 设置作用域名
	- `--always-auth` - 是否每次都需要验证
	- `--auth-type=legacy` - 设置验证策略

#### 源用户登出 - npm logout

```bash
# 源用户登出
npm logout [--registry=<url>] [--scope=<@scope>]
```

支持的args参数有
	- `--registry` - 设置源目标
	- `--scope` - 设置作用域名

#### 源个人资料配置 -  npm profile

```bash
npm profile get [--json|--parseable] [<property>]
npm profile set [--json|--parseable] <property> <value>
npm profile set password
npm profile enable-2fa [auth-and-writes|auth-only]
npm profile disable-2fa
```

#### 源token管理 - npm token

```bash
npm token list [--json|--parseable]
npm token create [--read-only] [--cidr=1.1.1.1/24,2.2.2.2/16]
npm token delete <id|token>
```

#### 缓存管理 - npm cache

```bash
# 操作缓存
# 加入指定包到本地缓存：这个命令主要目的是在内部使用npm是，可以提供一个显示增加数据到本地缓存的方式
npm cache add <tarball file>
npm cache add <folder>
npm cache add <tarball url>
npm cache add <name>@<version>
# 清理缓存：清除所有缓存目录的数据，npm@5需要强制--foce使用该命令
npm cache clean [<path>]

# 校验缓存目录内容，收集用不到的垃圾数据，验证缓存索引和缓存数据的完整性
npm cache clean verify
```

支持的args参数有
	- `--registry` - 设置源目标
	- `--scope` - 设置作用域名

#### npm配置管理 - npm config

```bash
# 管理npm配置

# 显示自定义的配置

npm config list 

# 显示所有的配置
npm config list [-l] 

# 编辑全局配置项
npm config edit

# 获取当前项目/全局配置项
npm config get <key> [-g|--global]
npm get <key> [-g|--global]

# 设置当前项目/全局配置项
npm config set <key> <value> [-g|--global]
npm set <key> <value> [-g|--global]

# 删除当前项目/全局配置项
npm config delete <key> [-g|--global]


```

支持的args参数有
	- `--json` - 输出json格式
	- `--scope` - 设置作用域名

#### npm环境自检 - npm doctor

```bash
# 检查你的环境
npm doctor
```

npm doctor运行一系列检查来确保你的npm安装有管理你的JavaScript包所需的东西。npm主要是一个独立的工具，但它有一些必须满足的基本要求：
- Node.js和git必须由npm执行
- 主npm源registry.npmjs.com或其他源可用
- npm使用的依赖安装目录node_modules（本地和全局）都存在，并且可以由当前用户写入
- npm缓存存在，并且其中的包压缩包没有损坏

#### bin文件目录 - npm bin

```bash 
# 显示npm的bin目录
npm bin [<args>]
```

支持的args参数有
	- `--global | -g ` - 全局模式

#### npm-config配置目录 - npm prefix

```bash 
# 输出npm-config配置目录
npm prefix [<args>]
```

支持的args参数有
	- `--global | -g ` - 全局模式

#### npm的依赖包安装目录 - npm root

```bash 
# 输出node_modules目录位置
npm bin [<args>]
```

支持的args参数有
	- `--global | -g ` - 全局模式

#### 已安装依赖包浏览 - npm explore

```bash 
# 浏览一个已安装包
# 浏览已安装的依赖包，运行指定依赖包的命令
npm explore <package> [ -- <commond>]

# 运行jquery的test脚本
npm explore jquery -- npm run test
```

支持的args参数有
	- `--shell` - 调用的shell终端
	
#### 已安装依赖包编辑 - npm edit

```bash 
# 编辑已安装的依赖包
npm edit [<args>]
```

支持的args参数有
	- `--editor` - 调用的编辑器

#### 帮助文档查看 - npm help

假如提供了一个主题，则会显示最适当的文档页面
假如提供多个术语，将运行`help-search`命令进行搜索

```bash 
# 查看指定术语的帮助文档
npm help[<terms...>]
```

支持的args参数有
	- `--viewer` - 调用的阅读器，可选值有"man" on Posix, "browser" on Windows

#### 帮助文档搜索 - npm help-search

```bash
# 根据提供的关键词搜索文档，输出一个结果列表，按相关性排序
# 如果只找一个主题，则直接显示详情内容
npm help-search <text>
```

支持的args参数有
- `--lone` - 显示扩展信息

#### 测试源网络是否连通 - npm ping

```bash
npm ping [<args>]
```

支持的args参数有
	- `--registry` - 设置源目标

## package.json

**package.json**必须是一个严格的json文件，而不仅仅是js里边的一个对象

如果你已有一份**package.json**文件，可以运行`npm install`指令，会自动根据相应的`dependencies`,`devDependencies`,`optionalDependencies`下载依赖包，并对`peerDependencies`字段的依赖包做出需要安装的提醒(npm@v3只提示)

### 环境变量

**package.json**的配置项可以可以通过`process.env`访问到，配置项使用`npm_package_version`和`NPM_PACKAGE_VERSION`前缀

### 最小化package.json

一个最小的**package.json**需要有`name`和`version`两个属性，这两个属性一起形成了一个npm模块的唯一标识符


- `name` - 名称
- `version` - 版本，遵循语义版本规范

```json
{
  "name": "my-awesome-package",
  "version": "1.0.0"
}
```

### 包初始化

可以通过`npm init`命令，通过一个交互提问的方式，初始化**package.json**文件，如果不需要提问式，可以使用`yes` flag，-y

```bash
# repl 交互提问方式
npm init

# 无提问方式
npm init --yes|-y
```

###  package.json 模板

你可以创建一个**package.json**模板文件，默认读取系统根目录下的文件，`~/.npm-init.js`
* 当使用了模板文件后，`npm init` 将不会执行交互式创建命令，直接使用模板

```js
module.exports = {
  customField: 'Custom Field',
  otherCustomField: 'This field is really cool'
}
```

### 我的模板

```js

module.exports = {
  "private": true, // 禁止将包误发布
  "name": "somename",
  "version": "1.0.0",
  "license": "MIT",
  "description": "",
  "keywords": [],
  "author": "lisfan <goolisfan@gmail.com> (https://www.npmjs.com/~lisfan)",
  "homepage": "https://lisfan.github.io/timer",
  "bugs": {
    "url": "https://github.com/npm/npm/issues"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/lisfan/timer.git"
  },
  "main": "src/index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

###  配置简介

主要了解以下字段(需要重新修正删减)
- name
- version 
- license
- description
- keywords
- author
- contributors
- homepage
- bugs
- repository
- main
- bin
- scripts
- dependencies
- devDependencies
- peerDependencies
- optionalDependencies
- bundleDependencies
- private
- engines
- os
- cpu
- config
- publishConfig
- man
- files
- preferGlobal
- deprecated


### 配置文件样本

```json
{
  "name": "@xkeshi/my-test2",
  "version": "2.2.0",
  "license": "MIT",
  "description": "a package manager for JavaScript",
  "keywords": [
    "package manager"
  ],
  "author": "lisfan <goolisfan@gmail.com> (https://www.npmjs.com/~lisfan)",
  "contributors": [
    {
      "name": "lisfan",
      "email": "goolisfan@gmail.com"
    }
  ],
  "homepage": "https://lisfan.github.io/timer",
  "bugs": {
    "url": "https://github.com/npm/npm/issues"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/lisfan/timer.git"
  },
  "main": "src/index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "bin": "./bin/cli.js",
  "deprecated": false,
  "dependencies": {
    "@~lisfan/validation": "^1.0.0"
  },
  "devDependencies": {
    "@~lisfan/logger": "^1.0.0"
  },
  "optionalDependencies": {
    "@~lisfan/storage": "^1.0.0"
  },
  "peerDependencies": {
    "localforage": "^3.0.0"
  },
  "bundleDependencies": [
    "@~lisfan/validation"
  ],
  "directories": {
    "bin": "./bin",
    "docs": "./docs",
    "src": "./src",
    "man": "./man"
  },
  "preferGlobal": true,
  "config": {
    "publishtest": false
  },
  "publishConfig": {},
  "man": [
    "/usr/local/lib/node_modules/npm/man/man7/semver.7"
  ],
  "files": {},
  "engines": {
    "node": "*"
  },
  "os": [],
  "cpu": []
}
```

### 配置大全

- **`name`** 名称 - string
> 名称中可以包含一个可选的scope前缀
> 强制规则
	- 不能超过214个字符
	- 命名时不能包含js、node、和url中需要转义的字符，不能以`.`和`_`为开头
	- 全小写
	- 不含不安全字符
	- 单个词，无空格
	- 允许破折号
> 建议
	- 不使用与核心`node`模块相同的名称
	- 不放置`js`和`node`在名称中
	- 在发布之前检查官网是否已存在同名包

- **`version`** 版本 - string
> 使用语义化版本

- `description` 描述 - string
> 便于npm search搜索
> Put a description in it. It's a string. This helps people discover your package, as it's listed in npm search.
> 如果没有描述字段，则会查找`README.md`或`README`的第一行代替

- `keywords` 关键词 - string[]
> 便于npm search搜索
> Put keywords in it. It's an array of strings. This helps people discover your package as it's listed in npm search.

- `homepage` 主页 - string
> The url to the project homepage.

- `bugs` 问题反馈 - object | string
> 提供bug反馈的方式，如果包提供了该字段，可以使用`npm bugs`进行反馈
> 
object格式可以参考如下配置
	{ 
		"url" : "https://github.com/owner/project/issues", 
		"email" : "project@hostname.com"
	}
如果只想提供url，可以直接使用字符串进行配置
> The url to your project's issue tracker and / or the email address to which issues should be reported. These are helpful for people who encounter issues with your package.
> You can specify either one or both values. If you want to provide only a url, you can specify the value for "bugs" as a simple string instead of an object.

- `license` 许可协议 - string
> 若不想授权，可以设置为`UNLICENSED`，同时设置`private: true`
> 你可以在https://spdx.org/licenses/这个地址查阅协议列表 。

- `author` 作者 - object | string
> 作者是单个人，作者是一个对象数据，一个`name`字段和可选的`url`和`email`
> 
{ "name" : "Barney Rubble"
, "email" : "b@rubble.com"
, "url" : "http://barnyrubble.tumblr.com/"
}
> 也可以将所有的信息放在一个是一个简短的字符串里
> "Barney Rubble <b@rubble.com> (http://barnyrubble.tumblr.com/)"
> The "author" is one person. "contributors" is an array of people. A "person" is an object with a "name" field and optionally "url" and "email", like this:
> Or you can shorten that all into a single string, and npm will parse it for you:

- `files` 

- `contributors` 贡献者 - object[] | string[]
> 作者是单个人，贡献者可以是多人
使用对象数据格式，包含必选的`name`和可选的`email`、`url`
{ "name" : "Barney Rubble"
, "email" : "b@rubble.com"
, "url" : "http://barnyrubble.tumblr.com/"
}
> 也可以使用一个固定格式的字符串，npm会解析它
"Barney Rubble <b@rubble.com> (http://barnyrubble.tumblr.com/)"

- `main` - 应用主入口 - string
> `main`字段是作为包的主入口一个模块ID，当你的包被用户下载安装后，模块被require时，该模块会返回`main`字段指定的模块导出对象
> require('name')引入模块时，模块将根据该字段，返回该模块的export对象
> The main field is a module ID that is the primary entry point to your program. That is, if your package is named foo, and a user installs it, and then does require("foo"), then your main module's exports object will be returned.
> This should be a module ID relative to the root of your package folder.
> For most modules, it makes the most sense to have a main script and often not much else.

- `bin` 命令行 - string
> 许多包希望有一个或多个可执行的命令安装到`PATH`，npm使其变的非常轻松
> A lot of packages have one or more executable files that they'd like to install into the PATH. npm makes this pretty easy (in fact, it uses this feature to install the "npm" executable.)
> 若想使用它，提供一个`bin`字段到你的**package.json**中。安装时，全局安装npm会创建一个符号链接到**prefix/bin**目录，本地安装会放置在**./node_modules/.bin/**
To use this, supply a bin field in your package.json which is a map of command name to local file name. On install, npm will symlink that file into prefix/bin for global installs, or ./node_modules/.bin/ for local installs.
> 举个例子，myapp有如下字段
> For example, myapp could have this:
**{ "bin" : { "myapp" : "./cli.js" } }**
> 所以，当你安装包后，它会从**cli.js**创建一个符号链接到bin目录
> So, when you install myapp, it'll create a symlink from the cli.js script to /usr/local/bin/myapp.
> 如果你只有一个可执行文件，并且它的名称就是包名，那你就可以提仅提供一个字符串格式，指向可执行脚本文件即可
> If you have a single executable, and its name should be the name of the package, then you can just supply it as a string. For example:
{ "name": "my-program"
, "version": "1.2.5"
, "bin": "./path/to/program" }
> 请确保你的文件是以 `#!/usr/bin/env`开头的
> Please make sure that your file(s) referenced in bin starts with #!/usr/bin/env node, otherwise the scripts are started without the node executable!

- `man` 提供man命令查看文件？ string | sting[]
- 指一个文件或者一组文件用于让`man`查找
> Specify either a single file or an array of filenames to put in place for the man program to find.
> 如果只有一个文件提供
> If only a single file is provided, then it's installed such that it is the result from man `<pkgname>`, regardless of its actual filename.
> would link the ./man/doc.1 file in such that it is the target for man foo
> If the filename doesn't start with the package name, then it's prefixed. So, this:
> { "name" : "foo"
, "version" : "1.2.3"
, "description" : "A packaged foo fooer for fooing foos"
, "main" : "foo.js"
, "man" : [ "./man/foo.1", "./man/bar.1" ]
}
> will create files to do man foo and man foo-bar.
> Man files must end with a number, and optionally a .gz suffix if they are compressed. The number dictates which man section the file is installed into.
> { "name" : "foo"
, "version" : "1.2.3"
, "description" : "A packaged foo fooer for fooing foos"
, "main" : "foo.js"
, "man" : [ "./man/foo.1", "./man/foo.2" ]
}
> will create entries for man foo and man 2 foo

- `directories` 模块结构 object
> 描述符合Common Js规范模块的结构，在未来会有用

- `repository` - 资源仓库 string | object
> 指定包的源文件有效存放地址。设置该项可以让贡献者参与你的项目
> 可以通过`npm docs`快速打开该资源指向的地址
> Specify the place where your code lives. This is helpful for people who want to contribute. If the git repo is on GitHub, then the npm docs command will be able to find you.
> 使用对象格式进行配置
> { 
	"type" : "git", 
	"url" : "https://github.com/npm/npm.git"
}
> 这个Url地址应该是一个公开的url，指向一个没有任何更改的版本管理系统。它不应该是一个项目的html页面
> The URL should be a publicly available (perhaps read-only) url that can be handed directly to a VCS program without any modification. It should not be a url to an html project page that you put in your browser. It's for computers.
> 针对GitHub, GitHub gist, Bitbucket, or GitLab仓库，你可以使用简结的语法
> {"repository": "gitlab:another/repo"}
> For GitHub, GitHub gist, Bitbucket, or GitLab repositories you can use the same shortcut syntax you use for npm install:

- `scripts` - 脚本钩子 - object
> `script`属性是一个是一个字典集合，包含了包的生命周期中各种各样的脚本命令
> The "scripts" property is a dictionary containing script commands that are run at various times in the lifecycle of your package. The key is the lifecycle event, and the value is the command to run at that point.

- `config` - script执行时的参数配置 - object
> 一个`config`可以用来设置scripts运行时的配置参数
> A "config" object can be used to set configuration parameters used in package scripts that persist across upgrades. For instance, if a package had the following:
> { "name" : "foo"
> , "config" : { "port" : "8080" } }

> 可以使用 `npm config set foo:port 8001` 进行设置，通过`process.env.npm_package_config_port`进行读取
> and then had a "start" command that then referenced the npm_package_config_port environment variable, then the user could override that by doing npm config set foo:port 8001.

- `dependencies` - 生产依赖 - object
> Dependencies 指定一个简单的对象去关联包的名称和版本方位。版本方位是包含一个或多个空格分隔的描述符。该值也可以是一个确定的tar包或git地址
> 将测试用例和编译转换模块只放置在`devDependencies`字段里
> Dependencies are specified in a simple object that maps a package name to a version range. The version range is a string which has one or more space-separated descriptors. Dependencies can also be identified with a tarball or git URL.
> **Please do not put test harnesses or transpilers in your dependencies object**. See **devDependencies**, below.
> 以下几种类型
	- URLs as Dependencies ：在版本范围的地方可以写一个url指向一个压缩包，模块安装的时候会把这个压缩包下载下来安装到模块本地。
	- Git URLs as Dependencies：git+https://user@hostname/project/blah.git#commit-ish 等，commit-ish 可以是任意标签，哈希值，或者可以检出的分支，默认是master分支。
	- GitHub URLs：支持github的 username/modulename 的写法，#后边可以加后缀写明分支hash或标签：
	- Local Paths：npm2.0.0版本以上可以提供一个本地路径来安装一个本地的模块，通过npm install xxx --save 来安装，格式如下：
		- ../foo/bar
		- ~/foo/bar
		- ./foo/bar
		- /foo/bar

- `devDependencies` - 开发环境依赖 - object
> 注意：当用户使用了该依赖包时，并不会下载此依赖包所需的开发依赖
> 这些模块会在npm link或者npm install的时候被安装，也可以像其他npm配置一样被管理，详见npm的config文档。
> 用户计划使用你的模块在他们的程序中，且他们也许不想提供一个下载或者扩展测试或文档时，你可以使用此字段
> If someone is planning on downloading and using your module in their program, then they probably don't want or need to download and build the external test or documentation framework that you use.
> 在构建发布之前自动运行`prepare`钩子脚本，用户就不需要去独立编译它们
> For build steps that are not platform-specific, such as compiling CoffeeScript or other languages to JavaScript, use the prepare script to do this, and make the required package a devDependency.
> The prepare script will be run before publishing, so that users can consume the functionality without requiring them to compile it themselves. In dev mode (ie, locally running npm install), it'll run this script as well, so that you can test it easily.

- `peerDependencies` - 同伴依赖 - object
> 提示用户需要安装的依赖（npm@v1和v2会自动安装同伴依赖，@v3只提示，不会主动安装，避免依赖地狱）
> In some cases, you want to express the compatibility of your package with a host tool or library, while not necessarily doing a require of this host. This is usually referred to as a plugin. Notably, your module may be exposing a specific interface, expected and specified by the host documentation.

- `bundleDependencies` - 依赖打包 - array
> 定义了一组发布包或者执行`npm pack`命令时时一起打包进包里的依赖
> This defines an array of package names that will be bundled when publishing the package.
> 当你需要本地保存包时，或者提供一个单一的文档供下载，你可以将依赖打包成一个tar包
In cases where you need to preserve npm packages locally or have them available through a single file download, you can bundle the packages in a tarball file by specifying the package names in the bundledDependencies array and executing npm pack.

- `optionalDependencies` - 可选的依赖 - object
> 类似`dependencies`，区别在于模块安装失败，也不会报错 
> `optionalDependencies`中的配置会覆盖`dependencies`中的配置，最好只在一个地方写。
> If a dependency can be used, but you would like npm to proceed if it cannot be found or fails to install, then you may put it in the optionalDependencies object. This is a map of package name to version or url, just like the dependencies object. The difference is that build failures do not cause installation to fail.

- `engines` - 指定node版本 - object
> 可以指定包所支持的node版本
> You can specify the version of node that your stuff works on:
> {
 "node" : ">=0.10.3 <0.12" ,
  "npm" : ">=0.10.3 <0.12" 
}

- `os` - 系统要求 - string[]
> 该值依据`process.platform`的值，进行设定白名单限制要求的系统，也可以使用`!`排除黑名单之名的系统。
> You can specify which operating systems your module will run on:

- `cpu` - 处理器架构 - string[]
> 该值依据`process.arch`的值，进行设定白名单限制要求的处理器架构，也可以使用`!`排除黑名单之名的处理器架构。

- `private` - 是否私有 - boolean
> 如果设置为true，将无法发布该包，这是为了防止一个私有模块被无意间发布出去
> If you set "private": true in your package.json, then npm will refuse to publish it.

- `publishConfig` - 发布配置?
> 这个配置是会在模块发布时用到的一些值的集合。如果你不想模块被默认被标记为最新的，或者默认发布到公共仓库，可以在这里配置tag或仓库地址。
> This is a set of config values that will be used at publish-time. It's especially handy if you want to set the tag, registry or access, so that you can ensure that a given package is not tagged with "latest", published to the global public registry or that a scoped module is private by default.
> Any config values can be overridden, but of course only "tag", "registry" and "access" probably matter for the purposes of publishing.

- `preferGlobal` - 更喜欢全局模式安装 - boolean
>  (已废弃)告诉用户更希望以全局方式安装该依赖

- `deprecated` 包是否已废弃 - boolean (未出现在官方文档中)
> 安装包时，提示用户该包已废弃（但仍然可以安装）

**npm会根据包中的内容默认设置如下值**

> 1. npm will default some values based on package contents.
"scripts": {"start": "node server.js"}
> If there is a server.js file in the root of your package, then npm will default the start command to node server.js.
2. "scripts":{"install": "node-gyp rebuild"}
If there is a binding.gyp file in the root of your package and you have not defined an install or preinstall script, npm will default the install command to compile using node-gyp.
3. "contributors": [...]
If there is an AUTHORS file in the root of your package, npm will treat each line as a Name `<email>` (url) format, where email and url are optional. Lines which start with a # or are blank, will be ignored.

#### 源上包的信息

通过`npm view <package>`命令可以查看源上的包信息，会多出一些信息

```json
{
  maintainers: [ // 包维护人员
    'moshengli <msl@xkeshi.com>'
  ],
  users: {}, // 用户列表？？？
  'dist-tags': { // 目标标记
    latest: '2.2.3'
  },
  time: { // 版本发布时间
    modified: '2017-10-13T10:37:10.000Z',
    created: '2017-09-29T07:58:53.233Z',
    '1.0.0': '2017-10-13T10:28:49.860Z'
  },
  versions: [ // 版本列表
    '1.0.0'
  ],
  gitHead: 'aff9dc29e4dbfc6f7c66322a737f054f01eedfe9', // git头
  dist: { // 输出信息
    shasum: '617cdb4282e6b6e654ddb39f9097b0a3a9091164',
    size: 9294,
    key: '/@xkeshi/my-test2/-/@xkeshi/my-test2-2.2.3.tgz',
    tarball: 'http://registry.fe.xkeshi.cn/@xkeshi/my-test2/download/@xkeshi/my-test2-2.2.3.tgz'
  },
  publish_time: 1507890529860 // 发布时间戳
}
```

## package-lock.json 和 npm-shrinkwrap.json

从概念上讲，`npm-install`的**输入**是一个**package.json**，而其**输出**是一个完整的**node_modules**目录树：表示你声明的依赖关系。
在一个**理想的世界**中，npm将像一个纯函数一样工作： 任何时候，npm都**package.json**应该产生完全相同的**node_modules**树。在某些情况下，确实如此。但在其他许多人中，npm无法做到这一点。这有几个原因：
- 不同版本的npm（或其他软件包管理器）可能已被用来安装软件包，每个软件包都使用稍微不同的**安装算法**。
- 自上次安装软件包以来，可能已经发布了新版本的直接semver范围的软件包，因此将使用更新的版本。
- 你的一个依赖关系的依赖可能已经发布了一个新的版本，即使你使用固定的依赖说明符（使用了1.2.3）
- 您安装的源已不再可用，或者允许版本突变（与主npm注册表不同），并且现在在相同版本号下存在不同版本的软件包。

> 为了防止这种潜在的问题，NPM使用**package-lock.json**或者**shrinkwrap.json**来锁定版本。一旦存在，任何未来的安装都将基于此文件而不是重新使用**package.json**的依赖版本 

**package-lock.json**可以通过`npm install`、`npm uninstall`、`npm update`自动创建和更新，用于管理包版本的锁定。

**npm-shrinkwrap.json**是一个通过`npm shrinkwrap`命令创建的文件。它与**package-lock.json**绝大部分特性相同，只有一个主要的区别：它不像**package-lock.json**在发布或打包时排除在外，**npm-shirnkwrap.json**在发布或打包时会被包裹进去；同时当包存在`npm-shrinkwrap.json`文件时，它所有的依赖将会安装在包目录的node_modules里，无论顶层是否已存在同版本的依赖

**npm-shirnkwrap.json**推荐的使用场景作为**命令行工具全局安装**或者作为**时确定依赖包的关系

**非常不建议**库的作者发布**package-lock.json**，它会阻止终端用户自已控制依赖的更新

注：当**package.json**和**npm-shrinkwrap.json**或**package-lock.json**产生冲突时，将以**package.json**文件的版本为准

你可以使用`--no-shrinkwrap`在安装依赖时排除在锁定版本外，或者使用`--no-save`，不将依赖写入到package.json文件


## npm-script

当你在本地安装 **带有cli命令**的工具包后，在scripts脚本中你无须以你能够从 `node_modules/.bin/<packagename>`这样的方式使用它，可以直接使用cli命令，它会自动访问`node_modules/.bin/<packagename>`的 bin 版本。

npm 支持在**package.json**中放置`scripts`属性，内部预定定了如下脚本

- `prepublish`：（废弃）通过运行`npm publish`或`npm pack`、`npm install`命令前 **BEFORE** 执行。
- `prepare`：（废弃）通过运行`npm publish`或`npm pack`命令前 **BEFORE** 执行。在`prepubish`钩子运行后，但在`prepublishOnly`钩子运行前
- `prepack`：通过运行`npm publish`或`npm pack`命令前 **BEFORE** 执行
- `postpack`：通过运行`npm publish`或`npm pack`命令前 **AFTER** 执行
- `prepublishOnly`：通过运行`npm publish`命令前 **BEFORE** 执行
- `publish`、`postpublish`：通过运行`npm publish`命令前 **AFTER** 执行
- `preinstall`：该模块被安装前**BEFORE** 执行
- `install`、`postinstall`：该模块被安装后**AFTER** 执行或通过运行`npm install`命令前 **BEFORE** 执行
- `preuninstall`、`uninstall`：该模块被卸载前**BEFORE** 执行通过运行`npm install`命令前 ***AFTER* 执行
- `postuninstall`：该模块被卸载后**AFTER** 执行
- `preversion`：通过运行`npm version <semver>`命令前 **BEFORE** 执行
- `version`：通过运行`npm version <semver>`命令后 **AFTER**，且commit 前
- `postversion`：通过运行`npm version <semver>`命令后 **AFTER**，且commit 后
- `pretest`、`test`、`posttest`：通过运行`npm test`命令
- `prestop`、`stop`、`poststop`：通过运行`npm stop`命令
- `prestart`、`start`、`poststart`：通过运行`npm start`命令
- `prerestart`、`restart`、`posrestart`：通过运行`npm restart`命令，若不存在，则先调用`stop`，再调用`start`脚本
- `preshrinkwrap`、`shrinkwrap`、`postshrinkwrap`：通过运行`npm shrinkwrap`命令

**包发布**和**包打包**和**包下载**命令会触发钩子脚本时机不同，具体表现为

- `npm publish` 命令会按顺序运行：`prepublish`、`prepare`、`prepublishOnly`、`prepack`、`postpack`、`publish`、`postpublish`
- `npm pack` 命令会按顺序运行：`prepublish`、`prepare`、`prepack`、`postpack`
- `npm install` 命令会按顺序运行：`preinstall`、`install`、`postinstall`、`prepublish`、`prepare`

**由于`prepublish`和`prepare`钩子在不同的命令中都会被使用到，所以不赞成使用，在npm@v5中已被废弃**

可以自定义任意的钩子，并通过`npm run-script <stage>`运行，同时有**pre** 和 **post**前缀匹配当前钩子名称的也会紧跟着运行(e.g. premyscript, myscript, postmyscript)。依赖包的`script`可以通过`npm explore <pkg> -- npm run <stage>.`运行
> Additionally, arbitrary scripts can be executed by running npm run-script `<stage>`. Pre and post commands with matching names will be run for those as well (e.g. premyscript, myscript, postmyscript). Scripts from dependencies can be run with npm explore `<pkg>` -- npm run `<stage>`.

### 我的配置

```
// 发布
1. git拉取
1. 提升版本
2. git提交
3. 发布
{    
	"publish-alpha": "npm version prerelease && npm publish --tag alpha"
}
// 
```

## .npmignore 配置

如果项目根目录中存在**.npmignore**文件（若不存在，则会查找**.gitignore**文件），NPM发布包时会根据[glob规则](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository#Ignoring-Files)排除不需要发布的文件及目录

因此为了git和npm进行协作，统一使用**.gitignore**文件来过滤文件

默认情况下，以下路径和文件将被忽略，**因此无需将它们.npmignore显式添加**

- ._*
- .DS_Store
- .git
- .hg
- .npmrc
- .lock-wscript
- .svn
- .wafpickle-*
- config.gypi
- CVS
- npm-debug.log
- package-lock.json

以下路径和文件不会被忽略，**因此添加它们.npmignore是无意义的**

- package.json
- README （及其变体）
- CHANGELOG （及其变体）
- LICENSE / LICENCE

### 我的配置

```bash
# project
# .project
upyun.config.js
server.config.js

# npm
node_modules/
npm-debug.log
# package-lock.json

# project ignores
# !.gitkeep
# *__temp
# /docs
# /examples_old
# /bin
# /coverage
# /lib
# /dist

# publish
# dist/
# public/
# build/
# zip

# temp files
.DS_Store
Thumbs.db
Desktop.ini

# vim swap files
*.sw*

# emacs temp files
*~
\#*#

# sublime text files
*.sublime*
*.*~*.TMP

# jetBrains IDE ignores
.idea


# visual studio code ignores
.vscode
```

## Q&A

1. `npm link`的有时与劣势
> 优势
- 模块不需要重复进行下载，节省空间，直接从全局包目录中获取
- 方便本地化调式插件

> 劣势
- 本地项目需要重新下载
- install一些别的插件时，会自动对link进来的包移除

2. 有哪些包可以提升使用`npm`的质量
> nrm - 自由切换源，如果指定了当前项目的源，则无法变更
> npx - 本地安装也可以使用带有bin脚本的命令行（npm@v5版本默认帮你安装了npx）


## 使用场景

1. 依赖包本地协同开发

2. 依赖包发布问题

3. 包发布时，自动执行一些脚本操作（script钩子的使用）
> 比如你用`CoffeeScript`写了一个类库，但你希望最终输出的是`JavaScript`，使用者不需要安装`CoffeeScript`依赖即可使用
> 这时，你就可以使用`prepublish`钩子（现在建议是使用`publishOnly`钩子）
> 让它在包发布前自动处理以下的可以包含如下任务
- 编译 `CoffeeScript` 源码到 `JavaScript`

4. 包被下载后，自动执行一些脚本操作
> 如(3)的使用场景，你想提供一个压缩后的版本，但又不希望将压缩过的版本打包在源包里，而是希望使用者安装了依赖包后，自动创建一个压缩版本。
> 这时，你就可以使用`install`钩子，安装后自动压缩