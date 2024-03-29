## CSS

层叠样式表
用于设置和布局网页

### 概述

#### What

在不使用CSS时，所有元素的样式都是浏览器的默认样式，这些样式固定不变

文档：是由标记语言组织起来的文本文件
- HTML是最常见的标记语言，当然，SVG 和 XML 也是标记语言
- 文档展示给用户实际是文档变成用户可用的文件
- 浏览器可以将文档在显示器等设备上可视化
- CSS 可以用来给文档添加样式、可用于创建布局、可以实现特效


#### 基本语法

```
h1 {
      color: color;
      font-size: 10em;
}
```
- 选择器
- 大括号 {}
- 属性-值对

#### 使用方式

- 外部样式表
  - 使用`.css`文件，使用`<link>`调用
- 内部样式表
  - `<head>` -> `<style>`
- 内联样式表
  - 元素使用`style`属性

注释：/* 开头  */ 结尾

#### 运行

1. 浏览器载入 HTML 文件
2. 将 HTML 文件转化成一个 DOM（Document Object Model），DOM 是文件在计算机内存中的表现形式
3. 接下来，浏览器会拉取该 HTML 相关的大部分资源，比如嵌入到页面的图片、视频和 CSS 样式
4. 浏览器拉取到 CSS 之后会进行解析，根据选择器的不同类型（比如 element、class、id 等等）把他们分到不同的“桶”中。浏览器基于它找到的不同的选择器，将不同的规则（基于选择器的规则，如元素选择器、类选择器、id 选择器等）应用在对应的 DOM 的节点中，并添加节点依赖的样式（这个中间步骤称为渲染树）
5. 上述的规则应用于渲染树之后，渲染树会依照应该出现的结构进行布局
6. 网页展示在屏幕上（这一步被称为着色）

DOM
- 树形结构
- 元素、属性、文字都对应树中的一个节点
- 节点之间有父子关系和兄弟关系

### 选择器

用来指定网页上我们想要格式化的HTML元素

#### What

是元素和其他部分组合起来告诉浏览器哪个HTML元素应当被当选为应用规则中的CSS属性值的方式
是CSS规则的第一部分

#### 选择器列表

当存在多个使用相同样式的 CSS 选择器，则这些选择器可以构成一个选择器列表

```
h1, .special {
  color: red;
}
```

注意：当选择器列表中的任何一个选择器无效（存在语法错误）时，则整个列表被忽略

#### 种类

- 类型（元素）选择器
直接选择标签作为选择器
`h1 {} `

- 类选择器
通过元素的class属性作为选择，使用 . 标识
`.box {} `

- id选择器
通过元素的id属性作为选择，使用 # 标识
`#unique {} `

- 标签属性选择器
根据一个元素上的某个标签的属性的存在或特定标签属性作为选择，使用 [] 标识
`a[title] {} `

- 伪类与伪元素选择器
包含了伪类和伪元素，用来格式化一个元素的特定状态
`a:hover {} ` 
`p::first-line {} `

- 运算符选择器
组合其他选择器，构建更复杂的选择元素，如 `>` 
`article > p {}`


### 列表

- border-bottom

border 的下面
`border-bottom: solid 1px #ca2100;`

- background-color

背景色

- transition

过渡
让属性动画的改变持续一段时间，而不是立即生效
`transition: height 200ms ease-in;`
三者分别代表属性、延迟时间、延迟变化函数（贝塞斯曲线）

- box-shadow

在元素的框架上添加阴影效果
`0 1px 2px rgba(0, 0, 0, 0.15);`


- display

设置元素是否被视为块或者内联元素以及用于子元素的布局

block：产生一个块级元素盒，在正常的流中，该元素之前和之后产生换行
- 块级元素，独占一行，不设置自己宽高的情况下，块级元素会默认填满父级的宽度
- 可以更改height和width
- 可以设置padding、margin，top、left等也可以产生边距效果
inline： 生成一个或多个内联元素盒，不会产生换行
- 使元素变成行内元素，可以与其他行内元素共享一行
- 不能更改元素的height、width值，大小由内容撑开
- 可以使用padding上下左右都生效，margin只有left和right产生边距效果


- hight & width

指定一个元素的宽高，默认情况下，这个属性决定的是内容区的大小，但是如果将

1. 值：30px、6em等，定义为一个绝对值
2. 百分比：75%，大小为外层的百分比大小
3. auto：浏览器自动计算并选择一个高度

- position

用于指定一个元素在文档中的定位方式，top、bottom、left、right属性决定了该元素的最终位置

- static：元素使用正常布局，即元素在文档中当前的布局位置，top等属性无效
- relative：元素先放置在未添加定位时的位置，再在不改变布局的情况下调整元素位置
- absolute： 元素被移除文档流，通过相对于最近的非static定位祖先元素的便宜来确定位置
- fixed：元素会被移出正常的文档流，并不为元素预留空间，元素的位置在屏幕滚动时不改变
- sticky

- top bottom left right

对于position是非static时，会对当前元素进行相对于相对元素的位置进行四个方向上的移动

- align-*

1. align-items

将所有直接子节点上的 align-self 值设置为一个组
flexbox 和 css 网格布局支持该属性，控制轴上项目的对其方式
center：元素在侧轴（竖向）居中，如果元素在侧轴上的高度高于其容器，那么在两个方向上溢出距离相同。

- box-sizing

定义了 user agent 应该如何计算一个元素的总宽度和总高度
对一个元素所设置的 width 与 height 只会应用到这个元素的内容区。如果这个元素有任何的 border 或 padding ，绘制到屏幕上时的盒子宽度和高度会加上设置的边框和内边距值。这意味着当你调整一个元素的宽度和高度时需要时刻注意到这个元素的边框和内边距
content-box：默认值，只会设置内容区的宽高
border-box：边框和内边距都会包含在宽高中
border-box不包含margin

- font-*

1. font-family

允许通过给定一个有先后顺序的，由字体名或者字体族名组成的列表来为选定的元素设置字体
示例：
`font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;`

2. font-size

指定字体大小

3. font-style

允许选择样式
normal：默认常规字体
italic：斜体
oblique： 斜体

4. font-weight

字体粗细程度，一般是normal 和 bold 和 lighter等