## Golang

The Go programming language

Go is an open source programming language that makes it easy to build simple, reliable, and efficient software.

目标：https://github.com/Qihoo360/poseidon

- 链接

官网：https://go.dev/
Github：https://github.com/golang/go
二进制文件：https://go.dev/dl/、
官方英文文档：https://go.dev/doc/
官方包查询：https://pkg.go.dev/

- 关键词

Google开源程序语言
底层支持并发机制
编译型语言
垃圾收集机制
静态类型语言
2012发布稳定版本
并发基于虚拟线程

- 适合

服务端开发
分布式系统/微服务
网络编程
区块链

### 开发环境

#### install

官方install文档：https://go-zh.org/doc/install

- Windows
- Linux
- Mac

#### 配置GOPATH

GOPATH 是一个环境变量，表明go项目的存放路径

- 只需要设置一个，所有项目都放到 GOPATH 的 src 目录下
- windows：需要和 go 二进制 bin 目录一样放到 path 环境变量中

#### 目录结构

GOPATH（顶层目录）

- bin（项目编译后的二进制文件）
- pkg（项目编译后的库文件）
- src（项目目录）
  - project_one
    - module_A
    - module_B
  - project_two
    - module_B
    - module_C

#### 编辑器

使用 VSCode

1. 需要用到的插件

- Go for Visual Studio Code

2. GO 开发工具包

3. 终端运行

#### 项目

1. 创建一个空文件夹用来放go源文件
2. 让依赖追踪你的代码
   - `go.mod`
   - `go mod init <module-path>`
   - 通过创建`go.mod`文件来进行go源文件的依赖包管理
3. 创建源文件，并写入代码
   - 声明package（即一组函数，使得文件在相同的目录下）
   - import，导入包，从标准库或其他模块导入
   - `main()`函数是一个包的默认执行函数，且package名默认为`main`
4. 运行代码
   - `go run .`

调用外部包的代码

- 在go的包查询（https://pkg.go.dev/）上查询一个包
- 找到包的路径即想要使用的函数
- 在自己的代码中导入对应路径的包即使用对应的函数
- Go 会添加 对应导入的模块，以及用于验证模块的`go.sum`文件
  - `go mod tidy`
  - 会出现远程获取不到的问题，需要修改代理服务器GoProxy
  - `go env -w GOPROXY=https://goproxy.cn`
  - 会将导入的包放在GOPATH下的pkg目录下
- 运行代码文件

### 基础

#### 特性

思想：Less can be more（大道至简）

优点：

- 自带gc
- 静态编译
- 无继承、多态、类
- 丰富的库
- 语法层支持并发
- 语法简介
- 交叉编译

特征：

- 自动回收
- 丰富的内置类型
- 函数多返回值
- 错误处理
- 匿名函数和闭包
- 类型和接口
- 并发编程
- 反射

文件名：源码以`.go`结尾


#### 创建GO模块

- 在一个模块中，你可以为一组离散且有用的功能收集一个或多个功能包
- Go代码被分组为包（package），包又被分组为模块（module）
- 模块指定运行代码需要的依赖，包括Go的版本以及其他模块的集合
- 当改进模块的功能后，需要发布新版本，调用你模块功能的开发者会获取最新版本的包并在发布生产前测试

创建可以被使用的模块

1. 准备一个目录，存放模块的源代码
2. 初始化模块：`go mod init <module_path>`
   - 包含模块名 和 支持的GO版本
   - 增加依赖时，`go.mod`会列出依赖的版本，保证构建的可见性以及版本的可控性
3. 编写模块需要的代码
   - 函数名大写开头，且同一个包中不能同名，作为函数可被导出的名称
   - `:=` 这个操作符可以在一行中快捷的声明和初始化一个变量
   - GO 声明变量类型在变量名的右边，例如 `var message string`

调用其他模块

1. 准备一个目录，存放调用的源代码
2. 初始化模块
3. 编写代码，调用其他模块的方法
4. 对于本地的模块，需要进行重定向操作
- `go mod edit -replace <other_module_name>=<modole_path>`
5. 跟踪模块：`go mod tidy`，同步依赖
   - `require module-path module-version` 将模块声明为当前模块的依赖项，并指定模块所需要的最低版本


https://go.dev/doc/tutorial/handle-errors
