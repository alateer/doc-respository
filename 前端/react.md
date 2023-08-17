## React

### 基础

前端组件化/模块化开发：
- 有利于团队协作
- 便于组件复用：提高效率、方便维护、减少冗余代码

组件划分：
- 普通业务划分
- 通用业务划分

#### create-react-app

脚手架：基于此创建React项目，默认把webpage的打包规则处理好了

安装脚手架
`npm install -g create-react-app`

创建项目
`create-react-app react-demo`

项目默认安装：
- react：React框架的核心
- react-dom：React视图渲染的核心【基于React构建WebApp】
  - react-native：构建和渲染App的
- react-scripts：自己对脚手架的一种分装，由它来打包
- web-vitals 性能监测工具

启动
`npm start`


#### 模式

React是Web前端框架

目前市场主流的前端框架
- React
- Vue
- Angular 【NG】】
- ...

主流思想：不去直接操作DOM，而是改为“数据驱动思想”

操作DOM：
- 消耗性能（主要原因是可能会造成DOM重排/重绘）
- 麻烦

数据驱动思想：
- 不直接操作DOM
- 操作数据（修改数据后，框架会按照相关数据，让页面重新渲染）
- 框架底层实现试图的渲染，也是基于操作DOM完成的
  - 构架一套 虚拟DOM -> 真是DOM 的渲染体系
  - 有效避免DOM重排
- 提高开发效率

React框架采用的MVC体系；Vue框架采用的MVVM体系
M：model 数据层
V：view 试图层
C：controller 控制层
VM：ViewModel 数据/视图监听层

MVC
- 数据+视图+控制
- 要按专业语法构建视图（页面）：基于jsx语法构建视图
- 构建数据层：视图中，需要动态处理的（不管是数据还是样式内容），都要有数据模型
- 控制层：在视图进行操作时，都需要去修改数据，然后渲染视图
- 数据驱动视图的渲染，单向驱动！

MVVC
- 数据+视图+监听
- 数据驱动视图渲染：监听数据的更新，让视图重新渲染
- 视图驱动数据的更改：监听表单元素内容的改变，自动更改相关数据
- 双向驱动


#### JSX

JSX React构建视图的基础
- JSX：javascript and xml（html）将JS和HTML标签混合使用
- 将js文件后缀改为`.jsx`即可
- 每一个构建的视图，只能有一个根节点
- 特殊标签（节点）：React.Fragment 空文档标签
  - `<> </>`
  - 既可以保证只有一个根节点、又不会新增HTML层级

{} 胡子语法特点
- 对于number 和 string， 什么值就渲染什么
- 对于boolean/null/undefined/Symbol/BigInt： 渲染的内容是空
- 普通对象不支持渲染，但存在特殊情况
  - JSX虚拟DOM对象
  - style行内样式必须是对象格式
- 数组对象：每一项都拿出来分别渲染（并不是变成字符串）
- 函数对象：不支持渲染，但是可以作为函数组件，用`<Component/>`方式渲染

style 样式
- 行内样式
样式必须写在{}里面，并且里面是对象的格式

底层原理之虚拟DOM

1. 将我们编写的JSX语法，编译为虚拟DOM对象【VirtualDOM】

虚拟DOM对象：框架内部构建的一套对象体系（对象的相关成员都是React内部规定的），基于这些属性描述出我们所构建视图中的DOM节点的相关特征

2. 将构建的虚拟DOM渲染为真实DOM

真实DOM：浏览器页面渲染出来的让我们看到的DOM元素
会基于ReactDOM中的render方法处理
- render：将虚拟DOM变为真实DOM

1. 补充
第一次渲染页面时是直接从VirtureDOM -> 真实DOM， 之后视图更新时，是会经过一次DOM-DIFF的对比，计算出补丁包PATCH（两次视图的差异部分），将PATCH渲染出来

#### 函数组件

组件化开发是现代化前端开发的核心
函数组件是React组件化的方式，通过构建函数方法来构建一个组件，通过函数
调用使用组件

- 每创建一个jsx文件，就相当于创建一个组件
- 函数组件需要返回一个虚拟DOM，即JSX结构
- 调用：基于ES6Module规范，导入创建的组件
- 使用：通过标签（单标签或双标签）使用
- 可以给组件设置（传递）属性（标题、样式、数据、类等）

渲染机制

1. 会基于 babel-preset-react-app将调用的组件转换为createElement格式
2. 将createElement方法执行，创建出一个virtualDOM对象
3. 基于root.render将virtualDOM变为真实DOM

可以基于函数调用将一些数据信息传递到函数组件中
双闭合调用可以传递子节点

属性props的处理

tips: 对象的规则设置
- 冻结
  - 冻结对象：Object.freeze(obj)
  - 检查是否被冻结：Object.isFrozen(obj)
  - 被冻结的对象，不能修改、新增、删除成员或成员值，不能给成员做劫持（defineProperty）
- 密封
  - 密封对象：Object.seal(obj)
  - 检测是否被密封：Object.isSealed(obj)
  - 被密封的对象：可以修改成员值，但也不能被删除、新增、劫持
- 扩展
  - 设置对象为不可扩展：Object.preventExtensions(obj)
  - 检测是否可扩展：Object.isExtensible(obj)
  - 被设置为不可扩展的对象：除了不能新增成员，其他都可以

- 调用组件时，传递的属性是只读的（冻结，包括密封和不可扩展）
- 传递的数据可以用在函数组件中使用，需要修改只能在父级调用组件时处理
- 可以通过把函数当做对象，设置静态私有属性方法，来给其设置属性的校验规则
  - defaultProps: 设置默认值
  - prop-types: 进行参数类型校验和是否必传等
  - 属性在进来时会先进行校验，如果失败，会在控制台告警

插槽

即使用双闭合的组件方式，为组件提供子组件的内容，但具体显示与否看调用方

- 使用插槽需要为子元素使用`slot`属性定义
- 可以在组件中将插槽信息从children的属性中获取出来
- 通过名称判断进行不同元素的渲染

静态组件

函数是“静态组件”
- 第一次渲染组件，把函数执行
  - 产生一个私有的上下文：EC（V）
  - 把解析出来的的props（含children）传递进来（冻结状态）
  - 对函数返回的JSX元素（虚拟DOM）进行渲染
- 当我们点击按钮的时候，会把绑定的小函数也执行
  - 修改上级上下文EC（V）中的变量
  - 私有变量值发生改变
  - 但是视图不会更新
=> 即，函数组件第一次渲染完毕后，组件中的内容，不会根据组件内的操作再更新，古称其为静态组件
=> 除非在父组件中再次执行这个函数组件（可以传递不同的属性信息）

动态组件

- 类组件
- Hooks组件（在函数组件中使用Hooks函数）

#### 类组件

属于动态组件

创建

创建一个构造函数（类），需要继承React.Component/PureComponent
- 要习惯使用ES6中的class创建类
- 必须给当前类设置一个render方法，在render方法中返回渲染的视图

继承

1. 基于call继承：React.Component.call(this)
2. 基于原型继承
3. 只给自己设置了constructor， 则需要执行 super()

初始化状态

### React Hooks 组件化开发

React组件分类

- 函数组件

- 类组件

#### Hook函数概述

- 基础 Hook
  - useState  使用状态管理
  - useEffect  使用周期函数
  - useContext  使用上下文信息

- 额外的 Hook
  - useReducer
  - useCallback
  - useMemo



#### useState

官方建议：需要多个状态，就将`useState`执行多次即可
```
let [supNum, setSupNum] = useState(10),
    [oppNum, setOppNum] = useState(5);
```

在 React18 中，我们基于 useState 创建出来的 “修改状态方法”，其执行是异步的
原理：
类组件中的 this.setState + 异步操作 + 更新队列

flushSync（react-dom中的）会立即执行更新队列

useState 自带了性能优化机制：
- 每次修改状态值的时候，会比较新的值和旧的值（基于Object.is比较），一样，则不会修改状态，也不会更新视图


#### useEffect

在函数组件中，使用声明周期函数
- useEffect(callback):
  - 在第一次渲染完毕后，执行 callback，等价于 componentDidMount
  - 在组件每一次更新完毕后，也会执行 callback，等价于 componentDidUpdate
- useEffect(callback, []):
  - 第一次渲染完毕后，执行 callback，之后组件更新不再渲染，等价于 componentDidMount
- useEffect(callback, [依赖的状态（多个状态）]):
  - 当依赖的状态（多个中的一个）发生改变时，也会触发 callback
  - 都没有发生改变时，不会触发 callback
- useEffect(callback(() => { return () => {} })):
  - callback 里面存在函数时，会等上一个组件生命周期结束（组件被释放）时执行callback

底层逻辑是使用一个 effect 链表（队列）

useEffect 必须在函数的最外层上下文中调用，不能嵌入到条件判断、循环语句中
可以在 callback 进行相关的语句判定

useEffect 如果设置了返回值，则返回值必须是一个函数（代表组件在销毁时触发）
可以在 callback 内部实现匿名函数或其他方式直接处理返回值，而不是将返回值作用到 callback 中

- useLayoutEffect
组件渲染步骤：
1. 基于 react-app 编译
2. 创建 virtualDOM
3. DOM-DIFF 渲染为 真实DOM

useEffect 会在第三步执行完之后再执行 effect 链表中的改变
useLayoutEffect 则会再第二步执行完就执行 effect 链表中更新，然后再渲染真实DOM
即：useLayoutEffect 会阻止浏览器渲染真实DOM，优先执行effect链表中的callback

#### useRef 和 useImperativeHandle

函数组件中使用Ref的方式
- 基于“ref={函数}”的方式，可以把创建的DOM元素（或者子组件的实例）赋值给 box
- 也可以基于 React.createRef 创建 ref对象来获取想要的内容
- 基于 useRef Hook函数，创建一个ref对象（只能在函数组件中使用）

useRef 和 createRef 
- 前者只能用在函数组件，后者也可以用在类组件
- 前者更新组件的时候，不会再创建新的REF对象，后者每一次更新都会重新创建一个Ref对象

React.forwardRef 可以将一个函数组件作为一个 Ref 对象返回
（正常的函数组件是不能调用另一个函数组件作为 Ref 对象的）

useImperativeHandle

获取Ref 子组件内部的状态和数据

#### useMemo 和 useCallback

函数组件的每一次更新，都是把函数重新执行一次
- 内部重新产生一个闭包
- -重新执行一次内部代码

useMemo 备忘录

对指定的状态指定其依赖的逻辑，只有这个状态改变时，对应的逻辑才会执行
用法：useMemo(callback, [指定的状态列表])
只有当指定的状态列表里面的状态发生改变时，才会执行callback

- 第一次渲染时，callback会执行
- callback的返回值会传递出来
- useMemo具备缓存
- 属于一个优化属性的Hook函数

useCallback

用法：useCallback(callback, [指定的状态列表])

- 组件第一次渲染，useCallback会执行，创建一个callback函数
- 组件后续的更新中，判断依赖的状态值是否改变，如果改变，则重新创建函数堆；如果没有改变，或状态列表为空，则始终使用第一次的callback函数堆
- 基于useCallback，可以始终获取第一次创建的函数引用

### 复合组件的通信方案

复合组件：父子、祖先后代、兄弟、平行等组件间的组合

父子组件通信方案：

1. 以父组件为主导，基于“属性”实现通信（只有父组件可以调用子组件，将属性传递给子组件）
2. 父组件基于属性“插槽”，可以将HTML结构传递给子组件
3. 父组件把方法基于属性传递给子组件
4. 父组件基于 ref 获取子组件实例（或子组件基于useImperativeHandle暴露数据和方法

单向数据流

1. 属性的传递方向是单向的

2. 生命周期函数的延续

- 第一次渲染
  - 父 willMount -> 父 render
  - 子 willMount -> 子 render -> 子 didMount
  - 父 didMount

- 组件更新
  - 父 shouldUpdate -> 父 willUpdate -> 父 render
  - 子 willReciveProps -> 子 shouldUpate -> 子 willUpdate -> 子 render -> 子 didUpdate
  - 父 didUpdate


方案二：上下文

基于上下文对象中，提供的Provider组件，用来：
- 向上下文中存储信息：状态和方法
- 当祖先更新的时候，render重新执行，会将最新的状态值，再次存储到上下文对象中

### 样式处理

vue中，会基于 `scoped` 为组件设置样式私有化，但是 react中并没有这样的机制

所以 react 存在一些样式私有化的处理方案

#### 内联样式

通过给组件写入内联样式，可以对组件的样式直接进行私有化处理
但是不利于样式的复用，样式冗余、样式和结构混合不够优雅

#### CSS样式表

基于样式表、样式类名这样的方式，但是需要人为有意识的、有规范的避免样式冲突问题
- 保证外层样式类名不冲突

#### CSS Modules

基于 CSS 的 Modules 实现样式私有化
样式写在 xxx.module.css 文件中，不能用less/sass/stylus这样的预编译语言了

CSS的规则都是全局的，任何一个样式规则，都对整个页面有效，产生局部作用于的唯一方法，就是使用一个独一无二的class名称

#### ReactJSS

一个工具包，'react-jss'，其内部存在创建css样式的方法：createUseStyles

基于 createUseStyles，构建组件需要的样式，返回结果是一个自定义的Hook函数
也是相当于js构建的css结构，会被预编译为相应的css，并且产生唯一的className，作为变量被使用
相较于CSSModules，具有可动态化的形式（预编译）

#### styled-components

流行 CSS-IN-JS 的模式，即把CSS像JS一样写，styled-components就是这样一个插件

### Redux

redux/react-redux 也是实现组件之间通信的技术插件，不管任何类型的组件，都可以基于这种方案实现通信
其实现的原理是： 基于公共状态管理

redux 是 JS 应用的状态管理容器，提供可预测的状态管理

相关的工具包：
- react-redux
- react toolkit
- redux devtools

真正的项目中，Redux 需要进行模块化拆分，即工程化

- 按照模块，将 reducer 进行单独的管理，每个模块都有自己的reducer
- 将各个模块的reducer都合并在一起，放在 store 中

- 派发行为标识宏管理
- actionCreator 对 action 进行同意创建和管理

react-redux
再react中便于使用redux

