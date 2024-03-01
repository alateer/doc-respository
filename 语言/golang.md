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

### 特性

思想：Less can be more（大道至简）
文件名：源码以`.go`结尾

#### 优点：

- 自带gc
- 静态编译
- 无继承、多态、类
- 丰富的库
- 语法层支持并发
- 语法简介
- 交叉编译

#### 特征：

- 自动回收
- 丰富的内置类型
- 函数多返回值
- 错误处理
- 匿名函数和闭包
- 类型和接口
- 并发编程
- 反射


### 创建GO模块

- 在一个模块中，你可以为一组离散且有用的功能收集一个或多个功能包
- Go代码被分组为包（package），包又被分组为模块（module）
- 模块指定运行代码需要的依赖，包括Go的版本以及其他模块的集合
- 当改进模块的功能后，需要发布新版本，调用你模块功能的开发者会获取最新版本的包并在发布生产前测试

#### 创建可以被使用的模块

1. 准备一个目录，存放模块的源代码
2. 初始化模块：`go mod init <module_path>`
   - 包含模块名 和 支持的GO版本
   - 增加依赖时，`go.mod`会列出依赖的版本，保证构建的可见性以及版本的可控性
3. 编写模块需要的代码
   - 函数名大写开头，且同一个包中不能同名，作为函数可被导出的名称
   - `:=` 这个操作符可以在一行中快捷的声明和初始化一个变量
   - GO 声明变量类型在变量名的右边，例如 `var message string`

#### 调用其他模块

1. 准备一个目录，存放调用的源代码
2. 初始化模块
3. 编写代码，调用其他模块的方法
4. 对于本地的模块，需要进行重定向操作
- `go mod edit -replace <other_module_name>=<modole_path>`
5. 跟踪模块：`go mod tidy`，同步依赖
   - `require module-path module-version` 将模块声明为当前模块的依赖项，并指定模块所需要的最低版本


#### 返回并处理错误

- 对于稳定的代码处理错误是基本的特性

1. 导入错误处理的包：`errors`，这是go的标准库中的错误包
2. 运行`errors.New("<>")`，返回错误信息
3. 在捕获异常的逻辑中引入：`log`包，进行错误信息的输出

#### 通过切片返回随机的数据

- 切片：`slice`类型，动态数组，数组大小会随添加和删除而变化
- 使用标注库的包：`math/rand`，可以使用随记的方法

#### 多参数输入和多参数输出

- 对于他人正在使用的已经发布的模块，不能改变原有函数的参数，可以通过新的函数进行兼容
- `map[key-type]value-type`，可以通过`make`函数进行初始化
- Loop 循环方结构：`for _, name := range names {}`，前者返回下标，后者返回值，`_`为空白标识符

#### 单元测试

- GO 内置了单元测试，使得测试更容易
- 标准库包：`testing`， 入参：`testing.T`类型，失败返回消息函数：`Fatalf`
- 命令：`go test <-v>`

1. 创建测试文件， 后缀为`_test.go`，这样GO的测试命令就可以认为该文件包含测试函数
2. 测试函数通常以`Test<function_name><test_function>`，指定具体要测试的功能，可以被测试命令捕获
3. 运行测试命令：`go test -v`，v 参数可以输出具体的测试信息
4. 当修改函数代码导致逻辑无法通过测试用例时，单元测试会失败并提示

#### 编译及运行应用

- `go run` 命令，当需要进行频繁的修改时，该命令是编译和运行代码的一个有效快捷方式，但这并不会产生二进制可执行文件
- `go build` 命令，会编译这个包，并附带它们的依赖，但不会安装结果
- `go install` 命令，编译并安装这个包
- `go list <-f '{{.Target}}'>` 命令，展示install中的可执行文件列表

1. 在一个module下运行 `go build` 命令
- 在该目录下产生一个 `.exe` 的二进制可执行文件
- 这时候依赖已经被构建了，如果依赖的其他模块修改了逻辑，就无法体现在可执行文件上
- 二进制文件可以被放到指定的二进制目录文件
2. 在一个module下运行 `go install` 命令
   - 需要配置环境变量，并设置 `go env <-w GOBIN=xx>` 中的 GOBIN 路径，来放 二进制可执行文件的地方
   - 默认在 GOPATH 的bin目录下存放，可以不做修改


### 多模块工作空间

https://go.dev/doc/tutorial/workspaces