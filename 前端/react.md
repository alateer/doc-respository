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


