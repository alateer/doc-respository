## Golang

The Go programming language

Go is an open source programming language that makes it easy to build simple, reliable, and efficient software.

- 链接

官网：https://go.dev/
Github：https://github.com/golang/go
二进制文件：https://go.dev/dl/、
官方英文文档：https://go.dev/doc/
官方包查询：https://pkg.go.dev/
awesome-go：https://github.com/avelino/awesome-go
golang开源项目：https://github.com/hackstoic/golang-open-source-projects
中文版awesome：https://github.com/shockerli/go-awesome
Gin 框架：https://gin-gonic.com/docs/
OpenSCRM：https://github.com/openscrm/api-server?tab=readme-ov-file
RxGo: https://github.com/ReactiveX/RxGo

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

可以在多个模块的构建代码工作空间，并管理运行和构建这多个模块

- `go get <path>` 命令，可以获取将一个依赖导入到当前模块
- `go work init <module_path>` 命令，创建一个`go.work`文件指定模块的工作空间，该模块会作为改工作空间的主模块
- `go work use <module_path>` 命令，将一个模块增加到当前工作空间

1. 创建一个工作空间目录，例如`xx-workspace`
2. 在工作空间创建一个模块目录，初始化模块并编写模块代码
3. 在工作空间目录通过`go work init <>`命令初始化模块的工作空间
4. 在工作空间通过`go run <modole_path>`来运行模块
5. 新创建一个模块，通过work use命令将新模块加入到工作空间
6. 工作空间中的模块间可以进行跨模块的调用

### 访问关系型数据库

- 标准库包：`database/sql`
- 包括连接数据库的种类和功能、执行约定、在程序中运行和启动等

1. 创建并初始化一个数据库连接模块，来管理与数据库连接相关的依赖以及调用
2. 在Mysql中创建数据表及填充数据
   - 创建数据库：`create database <database_name>`
   - 创建完成构建表和数据的`.sql`脚本文件
   - 使用 `source <sql_file_path>` 命令将脚本文件导入到当前数据库（注意，在windows上，盘符为/，末尾不带分号）
3. 查找并导入数据库驱动程序
   - 该驱动程序会通过包中的函数发出的请求转换为数据库可以理解的请求
   - 不同的数据库有不同的驱动程序，查找网址：`https://go.dev/wiki/SQLDrivers`
   - 在模块中创建main程序，并设为main package，导入对应的驱动
4. 获取数据库handler和连接
   - 通过一个数据库handler来获取数据库连接
   - 使用指向 sql.DB 结构的指针，表示对特定数据库的访问
   - 配置数据库连接信息，并开启数据库，判断连接是否成功
   - 使用 `go get .` 命令获取当前目录所需要的所有依赖
   - 设置环境变量，便于数据库配置去获取（目前设置不生效，直接注释掉）
   - 运行，返回连接成功即可
5. 数据操作
   - 定义数据库结构: `type <table_name> struct`
   - 编写查询逻辑进行数据的查询
   - 通过`db.Query("sql_statement")`查询多条数据
   - 通过`db.QueryRow("sql_statement")`查询一条数据
   - 通过 `defer rows.Close()` 来推迟关闭行源数据直到函数结束
   - 通过 `Rows.Scan` 将 rows 中的数据 加入到定义的数据结构中
   - Scan 会获取值的指针列表，通过 `&` 运算符可以将指针中的值传递到运算符对应的结构变量中，即通过指针写入来更新数据体结构
   - 调用查询使用即可
   - 数据操作包括查询、更新、删除、新增等
6. 相关练习代码
```go
package main

import (
	"fmt"
	"database/sql"
	// "os"
	"log"

	"github.com/go-sql-driver/mysql"
)

var db *sql.DB // database handler, is global variable

func main() {
	cfg := mysql.Config {
		// User: os.Getenv("DBUSER"),
		// Passwd: os.Getenv("DBPASS"),
		User: "root",
		Passwd: "123456",
		Net: "tcp",
		Addr: "127.0.0.1:3306",
		DBName: "recordings",
	}

	var err error
	db, err = sql.Open("mysql", cfg.FormatDSN())
	if err != nil {
		log.Fatal(`Open error: %v`, err)
	}

	pingErr := db.Ping()
	if pingErr != nil {
		log.Fatal(`Ping error: %v`, pingErr)
	}

	fmt.Println("Connected!")

	albums, err := albumsByArtist("John Coltrane")
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("Albums found: %v\n", albums)

	album, singleErr := albumByID(2)
	if singleErr != nil { 
		log.Fatal(singleErr)
	}
	fmt.Printf("Album found: %v\n", album)

	albID, err := addAlbum(Album{
		Title:  "The Modern Sound of Betty Carter",
		Artist: "Betty Carter",
		Price:  49.99,
	})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("ID of added album: %v\n", albID)
}

type Album struct {
	ID int64
	Title string
	Artist string
	Price float32
}

func albumsByArtist(name string) ([]Album, error) {
	var albums []Album

	rows, err := db.Query("SELECT * FROM album WHERE artist = ?", name)
	if err != nil {
		return nil, fmt.Errorf("albumsByArtist %q: %v", name, err)
	}

	defer rows.Close()

	for rows.Next() {
		var alb Album
		if err := rows.Scan(&alb.ID, &alb.Title, &alb.Artist, &alb.Price); err != nil {
			return nil, fmt.Errorf("albumsByArtist %q: %v", name, err) 
		}
		albums = append(albums, alb)
	}

	if  err := rows.Err(); err != nil {
		return nil, fmt.Errorf("albumsByArtist %q: %v", name, err)
	}

	return albums, nil
}

func albumByID(id int64) (Album, error) {
	var alb Album

	row := db.QueryRow("select * from album where id = ?", id)
	if err := row.Scan(&alb.ID, &alb.Title, &alb.Artist, &alb.Price); err != nil {
		if err == sql.ErrNoRows {
			return alb, fmt.Errorf("albumById %d: no such album", id)
		}
		return alb, fmt.Errorf("albumByID %q: %v", id, err)
	}

	return alb, nil
}

func addAlbum(alb Album) (int64, error) {
	result, err := db.Exec("insert into album (title, artist, price) values (?, ?, ?)", alb.Title, alb.Artist, alb.Price)
	if err != nil {
		return 0, fmt.Errorf("addAlbum: %v", err)
	}

	id, err := result.LastInsertId()
	if err != nil {
		return 0, fmt.Errorf("addAlbum, %v", err)
	}

	return id, nil
}
```

### 构建 RESTful API

- 使用 GO 和 Gin Web框架编写一个RESTful风格的服务端API
- Gin 简化了与构建web应用，包括web服务有关联的编码任务
- 通过Gin路由请求，获取请求细节，并将返回体处理为JSON
- 一个通用的API应该关联一个数据库

1. 构建并初始化一个 web service 的模块
2. 创建数据（数据库提供 或 JSON构建）
3. 构建API
   - 准备一个返回体结构
   - 请求参数类型：`gin.Context`
   - 请求返回体类型：`Context.IndentedJSON`，序列化json
4. 配置路由
   - 使用 `Default` 初始化 Gin router 
   - 使用 `GET(<api_path>, <function>)` 来映射路径和调用函数
   - 获取依赖
   - 运行
   - 请求api
5. 相关练习代码

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

type Album struct {
	ID string `json:"id"`
	Title string `json:"title"`
	Artist string `json:"artist"`
	Price float64 `json:"price"`
}

var albums = []Album {
    {ID: "1", Title: "Blue Train", Artist: "John Coltrane", Price: 56.99},
    {ID: "2", Title: "Jeru", Artist: "Gerry Mulligan", Price: 17.99},
    {ID: "3", Title: "Sarah Vaughan and Clifford Brown", Artist: "Sarah Vaughan", Price: 39.99},
}

func main() {
	router := gin.Default()
	router.GET("/albums", getAlbums)
	router.POST("/albums", postAlbums)
	router.GET("/albums/:id", getAlbumByID)

	router.Run("localhost:8080")
}

func getAlbums(c *gin.Context) {
	c.IndentedJSON(http.StatusOK, albums)
}

func postAlbums(c *gin.Context) {
	var newAlbum Album

	if err := c.BindJSON(&newAlbum); err != nil {
		return
	}

	albums = append(albums, newAlbum)
	c.IndentedJSON(http.StatusCreated, newAlbum)
}

func getAlbumByID(c *gin.Context) {
	id := c.Param("id")

	for _, album := range albums {
		if album.ID == id {
			c.IndentedJSON(http.StatusOK, album)
			return
		}
	}

	c.IndentedJSON(http.StatusNotFound, gin.H{"message": "album not fount"})
}

```

### 泛型（Generics）

对于泛型，你可以在调用方写入类型集合的函数或类型中声明和使用

定义泛型：`func func_name[K comparable, V int64 | float64](m map[K]V) V {}`
调用泛型：`func_name<[string, int64]>(maps)`

- 该泛型方法有两个类型参数，使用[]列出来
- comparable 为可比较的类型约束，GO允许任何类型的值可用作比较运算
- 通过 | 来处理允许返回的类型约束列表
- 调用时类型约束列表可以省略

声明类型约束：`type Number interface { int64 | float64 }`

- interface 是一个统一的风格结构

### 基础

#### GO内置

值类型

- 布尔型：bool
- 整型：int(32 or 64), int8, int16, int 32, int64
- 无符号整型：uint(32 or 64), uint8, uint16, uint 32, uint64
- 浮点型：float32, float64
- 字符型：string
- 复数：complex64, complex128
- 固定长度数组：array

引用类型（指针类型）

- 序列（可变）数组：slice
- 映射：map
- 管道：chan

函数（不需要导入）

- 追加数组元素：append
- 关闭channel：close
- 删除map中key对应的value：delete
- panic
- recover
- 返回complex实部：imag
- 返回complex虚部：real
- 分配内存（引用类型）：make
- 分配内存（值类型）：new
- 容量：cap
- 复制连接slice：copy
- 长度：len
- 底层打印函数：print|println

接口 error

#### 主要函数

init函数

`init` 函数用于包（package）的初始化

- 可以有多个init函数，但没有明确的执行顺序
- 不能被其他函数调用，在main函数执行前，自动调用

main函数

- Go默认入口函数

两者异同

- 定义时无参数和返回值，且go自动调用
- init可以用于任意包中，且可以重复
- main只能在main包中，且只有一个

#### 主要命令

`go env`：环境信息
`go run`：编译运行源码文件
`go get`：获取指定代码包及其依赖包，并编译和安装
`go build`：编译指定源码文件即其依赖
`go install`：编译并安装指定代码包及其依赖
`go clean`：清除执行其他命令时产生的一些文件和目录
`go doc`：打印实体文档
`go test`：单元测试
`go list`：列出指定代码包的信息
`go fix`：修补旧版本代码为新版本代码


#### 下划线_

特殊标识符，忽略结果

`import _ <package_path>`：不会导入整个包，只是执行包中的init函数，无法通过包名调用其他函数
在代码中使用，则是忽略该变量或者说作为占位符使用

#### 变量和常量

变量：var

- 变量保存的数据的内存地址
- 变量需要声明后才可以使用，同意作用域不支持重复声明
- 声明关键字及方式：`var name type`
- 变量声明时会默认进行初始话操作，每种类型的变量都有默认的初始值，可以指定初始值
- 类型可以在指定初始值时被推导出来
- 使用`:=`的方式完成简略的变量声明
- 匿名变量：`_`，用于占位，不存在重复声明

常量：const

- 恒定不变的值
- 声明是必须初始化赋值
- 常量计数器：`iota`，初始值为0

#### 数组Array

同一种数据类型 的固定长度的序列
定义：`var a [length]<type:int>`
数组是值类型，赋值和传参会赋值整个数组
多维数组：`var a [len][len]<type:int>`

#### 切片Slice

是数组的引用，属于引用类型
长度可以改变，即可变数组
定义：`var a []<type>`
make初始化切片：`var name []type = make([]type, len, cap)`，cap指容量
扩容方式：2的指数
string底层是一个byte的数组

#### 指针

不能偏移和运算，是安全指针
两个符号：`&` 取地址， `*` 根据地址取值
`pre := &v` 放在变量前面对变量进行取址操作
`pre := *v` 放在指针变量前面获取指针映射的值
`var v *type`，什么指针后未分配任何变量，即为空指针，不能直接赋值
通过`new` 和 `make` 来为空指针进行内存分配
- 两者都是用来进行内存分配的
- make只能用于引用类型（slice、map、channel）的初始化，返回引用类型本身
- new用于类型的内存分配，内存对应的值为类型零值，返回指向类型的指针

#### Map

key-value 数据结构，引用类型，需要初始化
定义：`map[KeyType]ValueType`
内存分配：`make(map[KeyType]ValueType, [cap])`，如果定义了但没有分配内存，进行赋值操作，会产生panic错误：assignment to entry in nil map
判断键是否存在：`value, ok := map[key]`，当值不存在时，使用值的默认值
遍历：`for k, v := range map {}`
删除key：`delete(map, key)`

#### 结构体

GO无类的概念，也无面向对象的特性，但可以通过结构体的嵌套和接口来完成相关的逻辑
类型定义：`type <my_type> <base_type>`
类型别名：`type TypeAlias = Type`，本质是同一个类型
两者区别：类型定义是包下的具体类型，类型别名被编译完成后依然是原有类型
结构体：`struct`，自定义数据类型，可以封装多个基本数据类型
定义结构体：`type <type_name> struct { filed_name filed_type }`
结构体只有在实例化时，才会分配内存，结构体本身就是一种类型
不进行实例化，则所有的成员都是默认值
使用临时数据结构的场景下可以使用匿名结构体


### 流程控制

#### 条件语句

if else

不支持三目运算

switch case

select case

#### 循环语句

for

range

Goto
Break
Continue

