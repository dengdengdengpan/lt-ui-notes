# Icon 组件

### 基本流程

图标是许多用户界面的重要组成部分，它能可视化地表达对象、动作和想法。如果使用得当，图标可以传达产品或行为的核心理念和意图，并且为用户界面带来很多好处，比如：减少用户的认知成本、节省屏幕空间、增强页面可读性和美感。LIcon 是一个语义化的矢量图标组件。

##### 需求分析

- 设置图标名称展示对应图标
- 设置颜色
- 设置大小
- 设置旋转
- 点击图标触发 `click` 事件

##### UI 设计

##### 代码开发

- 用户怎么使用

  ```vue
  <l-icon name="setting" />
  <l-icon name="plus" color="red" />
  <l-icon name="plus" :size="32" />
  <l-icon name="search" spin />
  ```

- `props`

  - `name`

  - `color`
  - `size`
  - `spin`

- `event`

  - `click`

##### 单元测试

使用 Vue Test Utils 和 Jest 进行单元测试。

### 知识点

##### SVG

SVG（Scalable Vector Graphics）全称是可缩放矢量图，是一种用于描述二维的矢量图，它基于 XML 标记语言。不同于 PNG、JPEG 这类基于像素处理图像的模式，SVG 是属于对图像形状的描述，所以它本质是一个文本文件，且体积较小，并且被无限放大都不会失真或降低质量，同时可以方便地修改内容。作为一个基于文本的开放网络标准，SVG能够优雅而简洁地渲染不同大小的图形，并和 CSS、DOM、JavaScript 等其他网络标准无缝衔接。

##### SVG Sprite

CSS Sprite 技术是指将所有的图都放在一张大图中，然后利用 CSS 的 `background` 属性精确定位出所需图的位置，这样做能减少请求个数。

SVG Sprite 技术则是：

- 使用 `symbol` 元素来标记 SVG 图标。

  ```html
  <svg>
      <symbol id="icon-1">
          <!-- 第1个图标 -->
      </symbol>
      <symbol id="icon-2">
          <!-- 第2个图标-->
      </symbol>
      <symbol id="icon-3">
          <!-- 第3个图标-->
      </symbol>
  </svg>
  ```

  其中，每一个 `symbol` 就表示一个 SVG 图标，但仅有上述代码是无法将 SVG 图标展示到页面上。

- 使用 `use` 元素来使用标记好的 SVG 图标。

  ```html
  <svg>
    <use xlink:href="#icon-1" />
  </svg>
  <svg>
    <use xlink:href="#icon-2" />
  </svg>
  <svg>
    <use xlink:href="#icon-3" />
  </svg>
  ```

  这样便能将 SVG 图标展示到页面上了。

在开发中，通常使用 svg-sprite-loader 生成 SVG Sprite，svgo 则用于 optimize SVG。

