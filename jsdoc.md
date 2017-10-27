
# ! JSDoc 学习笔记

@(002_前端开发)[javascript, jsdoc, esdoc, js 插件, 文档生成, 学习笔记, api 文档]

---

**修订历史**

| 修订版本 | 修订时间 | 修订人 | 描述 |
| ----- | ---------- | ------ | --- |
| 1.0.0 | 2016-04-01 | lisfan | 开篇 |
| 1.0.1 | 2016-06-16 | lisfan | 调整文档组织结构|
| 1.0.2 | 2016-06-18 | lisfan | 为每个标签分类和描述作用|
| 1.1.0 | 2017-10-20 | lisfan | 复习并修订 |

| 版本 | 学习深度 | 学习进度 | 学习范围 |
| ------ | --- | --- | ------- |
| v3.5.5 | 90% | 95% | 官方文档 和 css88翻译 |

## 目录

[TOC]

----

## 还需深入内容

- 开发JSDOC插件
- 开发JSDOC主题模板

## 问题列表

- [x]再回顾一次css88的翻译
- [x]有哪些JSDoc插件
- [x]哪些tag 支持定义虚拟注释
- [x]编译一个目录下所有的文件
- [x]整理一个使用了所有注释标签的源代码文件
- [x]寻找支持搜索的插件
	- 未找到很好的，考虑自已来，或者使用一个带搜索的模板
- [ ]让example支持markdown，同时又可以使用example标签
- [x]如何注释静态属性
- [ ]@param中的function类型支持简易方式指定的他的参数类型
- [ ]@todo支持md语法，同时调整他的展示方式
- [ ]扩展出@fixme，@xxx等标签
- [ ]扩展出http 相关的标签 @method，@query等标签
- [ ]为*.vue文件输出API文档
- [ ]如何与typescript，或者flow之间进行转换
- [ ]集成代码在线展示
- [ ]集成测试代码或者导出测试代码
- [ ]模板支持logo等替换
- [ ]模板中顶部显示import信息

## 参考引用

[git地址](https://github.com/jsdoc3/jsdoc)
[官方网站 & 文档](http://usejsdoc.org/)
[中文翻译教程](http://www.css88.com/doc/jsdoc/index.html)
[创作 JSDoc 插件](http://usejsdoc.org/about-plugins.html)

## 学习大纲

- [x]概要
- [x]安装
- [x]CLI选项
- [x]我的配置

## 产出成果

## 类似竞品
+ [documentjs](https://github.com/documentationjs/documentation)
+ [esdoc](https://github.com/esdoc/esdoc)

## 第三方扩展

### Templates

+ [jaguarjs-jsdoc](https://github.com/davidshimjs/jaguarjs-jsdoc)
+ [DocStrap](https://github.com/docstrap/docstrap) ([example](https://docstrap.github.io/docstrap))
+ [jsdoc3Template](https://github.com/DBCDK/jsdoc3Template)
  ([example](https://github.com/danyg/jsdoc3Template/wiki#wiki-screenshots))
+ [minami](https://github.com/Nijikokun/minami)
+ [docdash](https://github.com/clenemt/docdash) ([example](http://clenemt.github.io/docdash/))
+ [tui-jsdoc-template](https://github.com/nhnent/tui.jsdoc-template) ([example](https://nhnent.github.io/tui.jsdoc-template/latest/))

### Build Tools

+ [JSDoc Grunt plugin](https://github.com/krampstudio/grunt-jsdoc)
+ [JSDoc Gulp plugin](https://github.com/mlucool/gulp-jsdoc3)

### Other Tools

+ [jsdoc-to-markdown](https://github.com/jsdoc2md/jsdoc-to-markdown)
+ [Integrating GitBook with
JSDoc](https://medium.com/@kevinast/integrate-gitbook-jsdoc-974be8df6fb3)

## 基础

### 简介
JSDoc 3 是一个针对`JavaScript`语言的API文档生成工具，类似`javadoc`和`phpDocumentor`。你可以真接在源代码位置增加文档注释。JSDoc工具将扫描你的代码并为你生成**HTML**文档站点
> JSDoc 3 is an API documentation generator for JavaScript, similar to Javadoc or phpDocumentor. You add documentation comments directly to your source code, right alongside the code itself. The JSDoc tool will scan your source code and generate an HTML documentation website for you.

JSDoc 注释一般直接放置在需要生成文档的代码前面。JS解析器会按顺序解析每一个以`/**`开头的注释。**`/*`,`/***`**或者**大于3个星号**的注释将会被忽略。**这个特性允许你禁止解析注释块**
> JSDoc comments should generally be placed immediately before the code being documented. Each comment must start with a /** sequence in order to be recognized by the JSDoc parser. Comments beginning with /*, /***, or more than 3 stars will be ignored. This is a feature to allow you to suppress parsing of comment blocks.

块注释中简单的注释会被当成文档描述，特殊的`JSDoc 标签`使用时可以提供更多的信息
> Special "JSDoc tags" can be used to give more information.

### 块标签和内联标签

`JSDoc` 支持两种不同的标签

- 块标签 - Block tags, which are at the top level of a JSDoc comment.
- 内联标签  - Inline tags, which are within the text of a block tag or a description.

块标签，提供更详细的信息，内联标签通用用来链接到其他部分的文档，像html的`<a>`标签
> Block tags usually provide detailed information about your code, such as the parameters that a function accepts. Inline tags usually link to other parts of the documentation, similar to the anchor tag (`<a>`) in HTML.

块标签总是以符号`@`开头。除了JSDoc注释中最后一个块标记，每个块标签后面必须跟一个换行符

内联标签也以符号`@`开头。然而，内联标签及其文本必须用花括号`{ }`括起来，如`{@link description}`

### 名称路径

当引用一个在其他文档中的变量时，你必须提供一个**唯一标志符**来映射到该变量。名称路径提供了一种这样做的方法，并且消除**实例成员**，**静态成员**和**内部变量**之间的歧义。
> When referring to a JavaScript variable that is elsewhere in your documentation, you must provide a unique identifier that maps to that variable. A namepath provides a way to do so and disambiguate between instance members, static members and inner variables.

JSDoc 3 中名称路径的基本语法示例

```
myFunction
MyConstructor
MyConstructor#instanceMember
MyConstructor.staticMember
MyConstructor~innerMember // note that JSDoc 2 uses a dash
```

需要注意的是，如果一个构造函数有一个实例成员，这个实例成员也是一个构造器，那么你可以简单地将名称路径连接在一起，形成一个较长名路径名`Person#Idea#consider`，链接可与连接符号（#,.,~）任意组合使用
> Note that if a constructor has an instance member that is also a constructor, you can simply chain the namepaths together to form a longer namepath:

一个命名空间中的成员名称有带有特殊字符（哈希字符#号，破折号，双引号）。这种情况下，你需要这样引用这些名字：chat."#channel", chat."#channel"."op:announce-motd"，等等。在名称内部的双引号应该用反斜杠转义：`chat."#channel"."say-\"hello\""。
> Above is an example of a namespace with "unusual" characters in its member names (the hash character, dashes, even quotes). To refer to these you just need quote the names: chat."#channel", chat."#channel"."op:announce-motd", and so on. Internal quotes in names should be escaped with backslashes: chat."#channel"."say-\"hello\"".


### 路径前缀

namespacePath:
classPath:
modulePath

### 包含README文件
有两种方法可以将 **README** 文件中的信息合并到您的文档：

- 在项目根路径下，存在**README**的相关文件
- 使用`-R/--readme` 命令行选项运行JSDoc，指定文件的路径，他会忽略根路径下的`README`文件
- README文件可以使用任何名称和扩展名，但它必须是Markdown格式


```bash
# 在源路径中包含一个 README 文件
jsdoc path/to/js path/to/readme/README.md
```

```bash
# 使用 -R/--readme 选项
jsdoc --readme path/to/readme/README path/to/js
```

### 包含教程文档

JSDoc允许为你的API文档添加包含教程。你可以使用此功能来为您的API提供详细的使用说明，如“入门”指南或实现一个功能的一步一步的过程。

添加教程到您的API文档，可以通过`--tutorials` 或 `-u` 选项运行JSDoc，并提供JSDoc要搜索的教程目录。

```
# 使用 -u/--tutorials 选项
jsdoc -u path/to/tutorials path/to/js
```

JSDoc在指定目录中搜索具有以下扩展名的文件：

- `.htm`
- `.html`
- `.markdown` (转换 Markdown 为 HTML)
- `.md` (转换 Markdown 为 HTML)
- `.xhtml`
- `.xml` (作为HTML处理)

JSDoc还搜索目录下的`.json`格式的配置文件，创建教程的结构。这个文件包含**标题**、**排序**及子教程的配置

JSDoc给每个教程分配一个标识符。该标识符是不带扩展名的文件名。例如， `/path/to/tutorials/overview.md`分配的标识符是`overview`。

在教程文件中，您可以使用`{@link}`和`{@tutorial}`内联标签来链接到文档的其他部分。JSDoc将自动处理这些链接。

一个简单的配置对象可能是如下形式

```json
 {
     "tutorial1": { // 父教程的标志符
         "title": "Tutorial One", // 定义父教程的标题
         "children": { // 定义子教程
             "childA": { // 子教程的标志符
                 "title": "Child A"
             },
             "childB": {
                 "title": "Child B"
             }
         }
     },
     "tutorial2": {
         "title": "Tutorial Two"
     }
 }
```

### 使用package.json文件

**package.json**文件包含的信息对你的项目文档是很有用的，比如该项目的**名称**和**版本号**，在创建注释文档时，可以按项目名和版本号输出到对应的目录

有两种方法可以将 **Package** 文件中的信息合并到您的文档：

- 在项目根路径下，存在**Package**的相关文件
- 使用`-P/--package` 命令行选项运行JSDoc，指定文件的路径，他会忽略根路径下的**package.json**文件
- **package.json**文件可以使用任何名称和扩展名，但它必须是**npm**包格式

```bash
# 在源路径中包含一个 package.json 文件
jsdoc path/to/js path/to/readme/package.json
```

```bash
# 使用 -P/--package 选项：
jsdoc --package path/to/readme/package-other.json path/to/js
```

### 使用Markdown插件
JSDoc内置了一个**markdown**插件用来自动转换`markdown`格式文本到`HTML`文件。在JSDoc@v3.2.2版本后，**markdown**插件使用 `marked` 解析器
> JSDoc includes a Markdown plugin that automatically converts Markdown-formatted text to HTML. In JSDoc 3.2.2 and later, the Markdown plugin uses the marked Markdown parser.

Note: When you enable the Markdown plugin, be sure to include a leading asterisk on each line of your JSDoc comments. If you omit the leading asterisks, JSDoc's parser may remove asterisks that are used for Markdown formatting.

默认情况下，`markdown`插件只处理特定JSDoc标签的`markdown`文本。可以通过配置增加或排除其他标签的支持。
> By default, JSDoc looks for Markdown-formatted text in the following JSDoc tags:

- `@author`
- `@classdesc`
- `@description`
- `@param`
- `@property`
- `@returns`
- `@see`
- `@throws`

**Hard-wrapping text at line breaks （用换行符换行文本）**

> 默认情况下，Markdown插件不处理换行符换行的文本。这是因为，这是正常的JSDoc注释可以多行。如果您更喜欢处理换行符换行的文本，设置JSDoc配置文件中的`markdown.hardwrap`属性为`true`。此属性是在JSDoc3.4.0及更高版本中可用。

**Adding ID attributes to headings（添加ID属性到标题标签）**

> 默认情况下，Markdown插件不会给每个HTML标题标签添加`id`属性。想要标题标签文本自动添加`ID`属性，设置JSDoc配置文件`markdown.idInHeadings`属性为`true`。此属性是在JSDoc3.4.0及更高版本中可用。

### 配置JSDOC的默认模板

**Generating pretty-printed source files（生成适合打印的文档及是否生成文档中链接到源文件的链接）**
> 要禁用适合打印的文件，设置选项templates.default.outputSourceFiles为false。使用该选项也将删除文档中链接到源文件的连接。此选项在JSDoc3.3.0及更高版本上是可用的。

**Copying static files to the output directory(复制静态文件到输出目录)**
比如你可能在教程增加了一些图片
> templates.default.staticFiles.include：一个路径的数组，其内容应复制到输出目录。子目录也将被复制。
templates.default.staticFiles.exclude：路径的数组，指明这些 不 应该被复制到输出目录。
templates.default.staticFiles.includePattern：正则表达式，指明要复制的文件。如果这个属性没有被定义，所有的文件将被复制。
templates.default.staticFiles.excludePattern：正则表达式，说明哪些文件跳过(不复制)。如果这个属性没有被定义，什么都不会被跳过。

**Showing the current date in the page footer（在页脚显示当前日期）**
> 默认情况下，JSDoc的默认模板总是在生成文档的页脚显示当前日期。在JSDoc3.3.0或更高版本，可以通过设置选项templates.default.includeDate为false来忽略当前日期。

**Overriding the default template's layout file（重写默认模板的布局文件）**
> 默认的模板使用名为 layout.tmpl 的文件 指定每个生成文档的页面中的页眉和页脚。特别是，每个生产的文档页面会加载该文件定义了CSS和JavaScript文件。在JSDoc3.3.0或更高版本，可以指定使用自己的layout.tmpl文件，它允许你加载自己的自定义CSS和JavaScript文件，去除或替代，标准的文件。

要使用此功能，设置选项templates.default.layoutFile的路径到你的自定义布局文件。路径是相对于config.json文件，当前的工作目录，和JSDoc目录的相对路径，按照这个顺序。

### 用conf.json配置JSDoc

**Specifying input files(指定输入文件)**

**Incorporating command-line options into the configuration file(合并命令行选项到配置文件)**
在命令行中所提供的选项 优先级高于在conf.json中提供的选项。

**Tags and tag dictionaries(标签和标签字典)**

### 安装

```bash
# 安装JSDoc
npm install jsdoc

# 生成注释文档
jsdoc path/to/src

# 使用配置文件
jsdoc path/to/src -c path/to/conf.json
```

## CLI 命令

### 常用命令

- configure
- destination
- template
- tutorials
- readme
- encoding
- recurse

### 命令大全

| 选项 | 描述 |
| ------ | ----- |
| `-a/--access <value>` | 只显示特定access方法属性的标识符： `private`, `protected`, `public`, or `undefined`, 或者 `all`（表示所有的访问级别）。默认情况下， 显示除`private`标识符以外的所有标识符 |
| `-p/--private` | 包含项目名称，版本，和其他细节的package.json文件。默认为在源路径中找到的第一个package.json文件。 |
| `-P/--package` | 包含项目名称，版本，和其他细节的package.json文件。默认为在源路径中找到的第一个package.json文件 |
| `-c/--configure <value>` | JSDoc配置文件的路径。默认为安装JSDoc目录下的**conf.json**或**conf.json.EXAMPLE** |
| `-R/--readme <value>` | 用来包含到生成文档的**README.md**文件。默认为在源路径中找到的第一 **README.md** 文件 |
| `-u/--tutorials <value>` | 教程路径，JSDoc要搜索的目录 |
| `-t/--template <value>` | 用于生成输出文档的模板的路径。默认为**templates/default**，JSDoc内置的默认模板 |
| `-d/--destination <value>` | 输出生成文档的文件夹路径。JSDoc内置的Haruki模板，使用`console`将数据转储到控制台。默认为**./out** |
| `-r/--recurse <value>` | 扫描源文件和导览时递归到子目录 |
| `-e/--encoding <value>` | 当JSDoc阅读源代码时假定使用这个编码，默认为 utf8 |
| `-q/--query <value>` | 一个查询字符串用来解析和存储到全局变量env.opts.query中。示例：foo=bar&baz=true |
| `-h/--help <value>` | 显示JSDoc的命令行选项的信息，然后退出 |
| `-v/--version` | 显示JSDoc的版本号，然后退出 |
| `-X/--explain` | 以JSON格式转储所有的doclet到控制台，然后退出。 |
| `--verbose` | 解析的文件日志的详细信息到控制台JSDoc运行。默认为false。 |
| `--debug` | 打印日志信息，可以帮助调试JSDoc本身的问题 |
| `--pedantic	` | 将错误视为致命错误，将警告视为错误。默认为false |
| `--match <value>` | 只有运行测试，其名称中包含value |
| `--nocolor <value>` | 当运行测试时，在控制台输出信息不要使用的颜色。在Windows中，这个选项是默认启用的 |
| `-T/--test <value>` | 运行JSDoc的测试套件，并把结果打印到控制台 |

## 配置选项

### 我的配置

```json
{
  "opts": {
    "template": "node_modules/docdash",
    "readme": "README.md",
    "encoding": "utf8",
    "tutorials": "./tutorials",
    "destination": "./docs",
    "recurse": true,
    "verbose": true
  },
  "source": {
    "include": [
      "./src"
    ],
    "includePattern": ".+\\.js(doc)?$",
    "excludePattern": "(^|\\/|\\\\)_"
  },
  "tags": {
    "allowUnknownTags": true,
    "dictionaries": [
      "jsdoc"
    ]
  },
  "templates": {
    "cleverLinks": false,
    "monospaceLinks": false,
    "default": {
      "useLongnameInNav": true,
      "outputSourceFiles": true,
      "includeDate": true,
      "staticFiles": {
        "include": [
          "./tutorials/assets"
        ]
      }
    }
  },
  "plugins": [
    "plugins/markdown"
  ],
  "markdown": {
    "hardwrap": true,
    "idInHeadings": true,
    "tags": [
      "description",
      "param",
      "property",
      "returns",
      "throws",
      "todo",
      "example"
    ],
    "excludeTags": [
      "author",
      "classdesc",
      "see"
    ]
  },
  "docdash": {
    "static": false,
    "sort": false
  }
}
```

### 配置大全

```json
{
  "opts": { // JSDoc选项
    "template": "node_modules/minami", // 使用的主题模板
    "readme": "README.md", // 包含的readme文档
    "encoding": "utf8", // 输出的文档编码
    "tutorials": "./tutorials", // 包含的tutorial文档
    "destination": "./docs", // 输出地址
    "recurse": true, // 是否递归检索目录
    "verbose": true, //
  },
  "source": { // 需要生成的注释的文件及目录
	"include": [ "./src" ], // 需要生成的
    "exclude": [], // 排除生成的
    "includePattern": ".+\\.js(doc)?$", // 匹配的格式
    "excludePattern": "(^|\\/|\\\\)_" // 排除的格式
  },
  "tags": {
    "allowUnknownTags": true, // 是否支持未知标签
    "dictionaries": [ // 使用的标签字典库
      "jsdoc" // 使用标准JSDoc标签字
    ]
  },
  "templates": { // 模板配置
    "cleverLinks": false, //
    "monospaceLinks": false, // @link是否使用等宽字体
    "default": { // 默认配置
      "useLongnameInNav": true, // 是否使用长文件名
      "outputSourceFiles": true, // 是否在文档中导出源代码
      "includeDate": true, // 是否包含文档生成时间
      "staticFiles": {  // 静态资源输出到目标文件夹
        "include": [ // 包含的目录输出
          "./tutorials/assets"
        ],
        "exclude": [], // 排除的目录输出
        "includePattern": //, // 正则表达式，指明要复制的文件。如果这个属性没有被定义，所有的文件将被复制。
        "excludePattern": //, // 正则表达式，说明哪些文件跳过(不复制)。如果这个属性没有被定义，什么都不会被跳过。
      }
    }
  },
  "plugins": [
    "plugins/markdown" // 使用的插件
  ],
  "markdown": { // markdown插件的配置
    "hardwrap": true, // 使用硬换行
    "idInHeadings": true, // md标题使用ID
    "tags": [ // 支持md语法的标签
      "description",
      "param",
      "property",
      "returns",
      "throws",
      "todo",
      "example"
    ],
    "excludeTags": [ // 不支持md语法的标签
      "author",
      "classdesc",
      "see"
    ]
  },
  "docdash": { // docdash 模板自定义配置项
    "static": false, // 是否输出静态成员
    "sort": false // 是否对api名称进行排序
  }
}
```

## 注释标签

### ES 2015 Classes

ES 2015 classes在JSDoc3.4.0及更高版本支持。

JSDoc 描述一个遵循 ECMAScript 2015规范的类是很简单的。你并**不需要**使用诸如如`@class` 和 `@constructor`的标签来描述 ES 2015 classes，JSDoc通过分析你的代码会自动识别类和它们的构造函数。

当您使用 `extends`关键字来扩展现有的类的时候，你还是需要告诉JSDoc哪个类是你要扩展的，您可以使用 `@extends` (或 `@augments`) 标签。

### ES 2015 Modules

当你描述一个 ES 2015 module（模块）时，您将使用`@module` 标签来描述模块的标识符。

如果使用`@module`标签不带值，JSDoc会基于文件路径尝试猜测正确的模块标识符。

当您使用一个 JSDoc namepath（名称路径）从另一个JSDoc注释中引用一个模块，您必须添加前缀`module:`

### 基本信息组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @file/@overview/@fileoverview | 描述整个文件的功能，一般置于文件顶部标记 | `@description <some description>` | 
| @summary | 标识一个简短的描述，支持markdown语法 | `@summary <some copyright text>` | 
| @desc/@description | 描述API的作用，如果在**注释开始**处进行描述，则可省略此标签 | `@description <some description>` |
| @author | 作者信息，如果作者的后面跟着尖括号括起来的电子邮件地址， 默认模板将电子邮件地址转换为mailto:链接。 | `@author <name> [<emailAddress>]` |
| @version | 标记版本信息 | `@version <versionNumber>`
| @since | 标记该功能的添加版本 | `@since <versionDescription>`
| @copyright | 标记版权信息，一般与 @file 结合使用| `@copyright <some copyright text>`
| @license | 标记软件许可协议 | `@license <identifier>`
| @todo | 标记待做事项，支持markdown语法 | `@todo <Description>` | 
| @example | 提供代码示例 | `@example` | 
| @ignore | 忽略此注释块生成文档，该标签优先级大于其他标签，若出现在比较顶级作用范围的标签上，将会忽略其整个文档的定义 | `@ignore` | 
| @variation | 标记一个不同名称，用于区分具有相同名称的不同的对象 | `@variation <variationNumber>`
| @deprecated | 标记API已被废弃，描述中一般要指明是在什么版本被废弃的 | `@deprecated [<some text>]` | 
| @requires | 标记当前代码需要用到的外部模块，一般与@file配合使用 | `@requires <modulepath>` |
| @external/@host | 标记一个外部的类、命名空间或模块。它是虚拟注释。如为第三方库扩展了新的API，外部引用定义可以通过@extends、@memberof或者@function引用，如 `@function external:String#rot13` | `@external <NameOfExternal>` 

### 函数/方法组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @function/@func/@method | 标记为一个函数或方法（比如用于标记return回来是一个函数，比如用于函数表达式） | `@function [<FunctionName>]`
| @param | 标记函数或方法的参数描述，参数类型可以是一个内置的JavaScript类型，如string或Object，或是你代码中另一个标识符的JSDoc namepath（名称路径）。如果你已经在这namepath（名称路径）上为标识符添加了描述，JSDoc会自动链接到该标识符的文档，具体用法参考[官方例子](http://usejsdoc.org/tags-param.html) | `@param [{type}] [<name>] - [<description>]` | 
| @this | 标记 `this` 的引用指向 | `@this <namePath>`
| @return/@returns | 标记返回值的描述，语法和@param类似。| `@return [{type}] -  [<description>]`
| @throws/@exception | 标记抛出错误方法，支持多个@throws标签 | `@throws {<type>} free-form description`
| @callback | 定义一个回调函数描述，作为其他参数的类型，就像@typedef标签自定义类型那样使用它。支持虚拟注释 | `@callback <namepath>`
| @async | 标记一个异步方法 | `@async`
| @generator | ... | `@generator`
| @yields/@yield | ... | `@yields [{type}] [description]`

### 事件组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @event | 描述一个事件 | `@event <className>#[event:]<eventName>`，事件相关的`event:`标签都是可以省略的
| @fires/@emits | 标记触发一个事件 | `@fires <className>#[event:]<eventName>` | 
| @listens | 标记监听一个事件 | `@listens <className>#[event:]<eventName>` | 

### 变量/属性组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @const/@constant | 标记为常量 | `@constant [<type> <name>]` | 
| @member/@var | 标记为对象的一个成员，**支持虚拟注释** | `@member [<type>] [<name>]`
| @default/@defaultvalue | 标记默认值（比较适合标记属性或常量） | `@default [<some value>]`
| @type | 标识变量或参数的数据类型 | `@type {typeName}` |
| @enum | 描述一个**静态属性值的全部相同**的集合。枚举对象中的所有key，除了枚举自己的描述注释之外，属性都记录在容器内部的注释中。通常这种标签是与@readonly结合使用 | `@enum [<type>]`。若其中某个枚举值数据类型不同（通常情况下不应该出现该情况），可使用@type额外指定 | 
| @property/@prop | 标记一个**对象的字面量**，适用于描述属性的集合 | `@property {<Type>} <Description>`
| @typedef | 标识一个自定义的类型，特别是你想反复引用他的时候。使用@callback标签表明回调函数的类型。 | `@typedef [<type>] <namepath>`

### 作用域组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @global | 标记为全局，忽略在源文件中的实际作用范围 | `@global` | 
| @inner | 标记为内部	成员，这意味着它可以通过 "Parent~Child" 被引用。 | `@inner` | 
| @static | 标记为一个静态成员(比如不需要实例即可使用) | `@static` | 
| @readonly | 标记为只读 | `@readonly` | 

### 访问权限组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @access | 标记该成员的访问级别（私有 private，公共 public，或保护 protected） | `@access [package\|private\|protected\|public]`，`@access private 等价于 @private`，`@access protected 等价于 @protected`，`@access public 等价于 @public`，`@access package 等价于 @package`， | 
| @private | 标记为私有，@private标签不被子成员继承，**私有成员不会显示在生成的输出文档**中，除非通过`-p`/`--private`命令行选项运行JSDoc。私有成员不会被继承类继承 | `@private` | 
| @protected | 标记为受保护的，受保护的成员只能在被继承的子类中或在模块内部可以访问。且不应该被覆盖的 | `@protected` | 
| @public | 标记为公开，默认都是公开的 | `@public` | 
| @package | 标记为包私有 | `@package` | 

### 定义类型组

| 标签 | 作用 | 示例 |
| - | - | - | 
| @namespace | 标记为命名空间变量 | `@namespace [{<type>}] <SomeName>]`
| @name | 标记一个对象的名称，强制使用该名称，忽略代码里的实际名称，通过使用@name标签，你告诉JSDoc忽略实际代码，隔离您的文档注释。当您使用@name标签，你必须提供额外的标签，来告诉JSDoc什么样的标识符将被文档化;该标识符是否是另一个标识符的成员;等等。如果不提供这些信息，标识符将不会被正确文档化。在许多情况下，最好是使用@alias 标签 代替，这个标签只是改变了标识符的名称，但是保留了标识符的其他信息。 | `@name <namePath>`
| @alias | 使用别名来描述这个成员（会替换原来的名称在全文中的定义，它仅仅只是换了个名称，除此之外没有干啥） | `@alias [aliasNamepath]` | 
| @lend | 将一个**字面量对象**的所有属性标记(借给)为某个（类或模块）的成员 | @lends [[Link]]，使用`@lends Person`表示标记为**类的原型对象的静态成员**，如使用`@lends Person.prototype`表示标记为**类的原型对象的实例成员** | 
| @constructs | 标记某个函数为构造函数，作用有点同@class？需要配合@lends使用？ | `@constructs [<name>]` | 
| @kind | 标记类型，**一般不用这个标签** | `@kind <kindName>` , kindName可以是`class`、`constant`、`event`、`external`、`file`、`function`、`member`、`mixin`、`module`、`namespace`、`typedef` | 

### 定义类组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @class/@constructor | 标记类的构造函数 | `@class [<type> <name>]` | 
| @classdesc | 为类提供一个描述，与@description不同，@classdesc的描述目的更明确，且应该与@class结合使用 | `@classdesc <some description>`
| @instance | 标记为一个原型成员，可以绑定到class或者namespace上 | `@instance`
| @memberof，@memberof! | 标记成员属于哪个父类的实例成员，默认情况下，@memberof标签标注的标识符是静态成员，可以使用语法："@memberof ClassName.prototype" 或者 "@memberof ClassName#"。或明确标注@inner或 @instance标签。“强制的”@memberof标签，@memberof！强制对象被记录为属于特定的父级标识符，即使它有不同的父级标识符。 | `@memberof <parentNamepath>`，`@memberof! <parentNamepath>` |

### 继承类组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @extends/@augments | 标记一个类继承自基类，如果继承自多个父类，并且多个父类有同名的成员，JSDoc会自动使用最后一个父类同名成员注释 | `@extends <namepath>` | 
| @abstract/@virtual | 父类中的成员必须在继承类中实现（或重写） | `@abstract` | 
| @inheritdoc | 当继承子类时，同名方法自动使用父类注释 | `@inheritdoc` | 
| @override | 标记继承子类，覆盖父类方法，但如果指定了该标记，**则注释会引用父类的**，若你想独立写注释，则不填加@override即可 | `@override` | 

### 类接口组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @interface | 标记为类接口，被其他类实现，该标签应该添加到顶层标识符，不需要添加到每个需要实现的接口上。支持虚拟注释 | `@interface [<name>]` | 
| @implements | 标记为实现了某个类的接口，该标签应该添加到顶层标识符，不需要添加到每个需要实现的接口上。且会自动引用接口API的说明 | `@implements [<class-name>]` | 

### 混合组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @mixin | 标记该对象可能需要被混入到其他对象上 | `@mixin [<MixinName>]`
| @mixes | 标记一个对象混合了另一个对象的所有成员，会自动增加被混入对象的API文档到该对象上 | `@mixes <OtherObjectPath>`

### 模块组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @module | 标明当前文件模块，在这个文件中的所有成员将被默认为属于此模块，除非另外标明 | `@module [[{<type>}] <moduleName>]`
| @exports | 标记为模块导出成员，一般情况下会自动检查模块导出语法，目前该标记作用极其类似module，**未确定有什么不同** | `@exports <moduleName>`

### 引用组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @see | 标记更多详细信息的浏览地址，链接到一个参考位置，值可以是路径名称或者外链地址 | `@see <namepathOrDescription>`
| @borrows | 一个名称路径下的属性/函数描述信息直接复制另一处的名称路径下属性/函数，注**该值*放置在顶部，而不是放在某个方法上* | `@borrows <that namepath> as <this namepath>` | 

### 内联组

| 标签 | 作用 | 示例 | 
| - | - | - | 
| @link,`@linkcode`,`@linkplain` | 内联标签，标识一个链接，支持多种语法 | 1. `{@link namepathOrURL}` 2. `[link text]{@link namepathOrURL}` 3. `{@link namepathOrURL|link text}` 4. `{@link namepathOrURL link text (after the first space)}` | 
| @tutorial | 内联标签，插入一个指向向导教程的链接 | 1. `{@tutorial tutorialID}` 2. `[link text]{@tutorial tutorialID}` 3. `{@tutorial tutorialID|link text}` 4. `{@tutorial tutorialID link text (after the first space)}` | 


## Q&A
## 使用场景
## 开发者