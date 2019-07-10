# 一.基本介绍
Node.js 由三部分组成：ECMAScript 核心 + 全局成员 + 模块成员 

# 二.模块
在 Node 环境中，一个.js文件就称之为一个模块（module）。

模块化的优点：提高代码的可维护性、可重用性高、可以避免函数名和变量名冲突。

模块成员，又可以分为三大类： 核心模块、第三方模块、用户自定义模块。

## 1.核心模块
- 随着 Node.js 的安装包，一同安装到本地的模块，叫做核心模块；
- 例如：fs，path 等模块，都是由 Node.js 官方提供的核心模块；
- 只要在计算机中，安装了 Node 应用程序，那么计算机中就已经安装了所有的核心模块；
- 使用核心模块：require('核心模块标识符'）

## 2.第三方模块
- 一些非官方提供的模块，叫做第三方模块；
- 需要使用某些第三方模块，必须去一个叫做 NPM 的网站上搜索并下载才能使用；
- 先从 npm 官网上下载指定的第三方模块，使用 require('第三方模块的名称标识符')来导入这个模块，根据第三方模块的官方文档，尝试使用

## 3.用户自定义模块
- 程序员在自己项目中写的 Javascript 文件，就叫做 用户自定义模块

# 四. Node 模块机制
- 以模块(文件)来划分功能、组织代码和代码复用；
- 用包规范来管理应用和可复用组件；
- 用npm工具和网站进行开源社区代码共享

# 五. Node 模块模型
- 核心模块（C++ 写的，可以直接拿来用的）
    - C/C++ 内建模块，最底层的模块。提供 API 给 Javascript 核心模块或其他 Javascript 文件模块调用(process.binding())
    - Javascript 核心模块，存放在 Node 安装目录的 lib 下，为 C/C++ 内建模块提供封装和桥接
- 文件模块
    - C++ 扩展模块，为了运行效率和特殊目的按照 Node 规范编译，扩展名为 node，用process.dlopen()加载执行（动态连结库）
    - Javascript文件模块，由第三方或用户自行编写的模块

# 六.常用的 Node 核心模块
- console：控制台输入输出
- fs：与文件系统交互（在不同操作系统下是不一样的，node 底层有个机制封装，可以使我们用的一样，实现跨平台）
- fttp：提供 HTTP 服务器功能
- net：有很多 C++ 的库 提供 TCP/IP 网络功能
- os：提供了一些操作系统相关的实用方法，比如取 cpu、内存的信息
- path：一般和 os 合起来用 提供了一些操作系统相关的实用方法
- url：解析 URL
- querystring：解析 URL 的查询字符串
- crypto：提供加密和解密功能，基本上是对 OpenSSL 的包装

# 七.模块对象
- 模块对象( module )实际上也是一个文件，表示当前模块文件，是一个 js 对象。模块对象不是全局的，而是每个模块唯一的
- 模块对象的属性
    - module.id，模块的标识符，通常是完全解析后的文件名
    - module.loaded 模块是否已经加载完成，或正在加载中 js 运行时要先运行模块中的代码，加载完成后再执行其他的
    - filename 模块完全解析后的文件名
    - parent 调用该模块的模块
    - children 被该模块引用的模块对象
    - exports 模块的导出对象，默认为空对象

# 八.模块的导出
- module.exports，模块的导出对象，默认为空对象 例子 exptest
    - exports 变量是 module.exports 的快捷方式，module.exports.f=…可以被更简洁地写成 exports.f=…
    - exports变量直接赋值（exports=…）会使之不再是 module.exports 的快捷方式，导致不能被导出
    - 为避免出错，模块的导出都用 module.exports ，不用 exports

# 九.模块引用
- 使用 require 方法来指定加载模块，方法的参数是模块的标识符
- 模块标识符包括
    - 核心模块，如 http, fs, path 等（核心模块不需要写路径，默认路径）
    - 文件模块，可以省略文件扩展名
        - 以.或..开始的相对路径
        - 绝对路径
    - 安装包模块，如 express, colors
        - 在全局（npm list -global）或当前应用的 node_modules 中寻找目录
        - 在 package.json 文件中，寻找 main 属性所指明的模块入口文件
        - 没有 package.json 文件，以 index.js 为模块入口文件
- require.resolve()可用来解析模块标识符的绝对路径。

# 十.模块缓存

- 模块在第一次 require 后会被缓存，多次 require 不会导致模块的代码被执行多次
- Node 模块的缓存不同于浏览器的 js 缓存，浏览器只缓存文件，Node模块缓存的是编译和执行之后的对象
- require.cache 对象代表 Node 模块缓存区，缓存的模块以属性的方式加入该对象，可以用 require.cache[‘模块标识符’]来访问具体的缓存模块，也可以delete该缓存

# 十一.Node包规范
- 英文名叫做 Packages，包是在模块基础上更深一步的抽象
- 包的目的：方便分发和推广基于 CommonJS 规范实现的 应用程序 或 类库
- 包可以看作是 模块、代码 和 其它资源 组合起来形成的 独立作用域

# 十二.包结构
- 包都要以一个单独的目录而存在
- package.json 必须在包的顶层目录下
- package.json 文件必须符合 JSON 格式，并且必须包含如下三个属性：name, version, main
    - name: 包名，npm install依赖此名称
    - version: 包的版本号 版本号为a,b,c的形式，其中 a 是大版本号，b 是小版本号，c 是补丁号
    - main: 表示包的入口文件，包输出主入口模块的 ID，当包被 require 时，返回的就是这个模块的导出
    - description：项目描述，npm search 会用到
    - keywords：关键字，npm search 会用到
    - author，contributors：author 是一个人，contributors 是一组人
    - dependents：当前包所依赖的其他包和包的版本
    - scripts：指定了运行脚本命令的 npm 命令行缩写
- 二进制文件应该在 bin 目录下
- javaScript 代码应该在 lib 目录下
- 文档应该在 doc 目录下
- 单元测试应该在 test 目录下
- Node.js 对包要求并没有那么严格，只要顶层目录下有 package.json，并符合基本规范即可






