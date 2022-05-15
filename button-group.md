# ButtonGroup 组件

### 基本流程

##### 需求分析

按钮组是一种组合，它将多个按钮放置在一起用以进行多项操作。

##### UI 设计

##### 代码开发

- 用户怎么使用

  ```vue
  <l-button-group>
    <l-button icon="left">上一页</l-button>
    <l-button icon="right" icon-position="right">下一页</l-button>
  </l-button-group>
  ```

- 要解决以下几个问题：

  1. 按钮组也应该是 `inline-flex`。
  2. 位于按钮组中间的按钮的左右 `border` 会显得似 2px 一样。
  3. 位于按钮组中间的按钮不应该有 `border-raidus`。
  4. 当按钮类型是 `secondary`、`primary` 时，要设置位于中间的按钮的 `border-color`。
  5. 当用户使用 ButtonGroup 组件时，如果其直接子组件不是 Button 应该给予提示。

##### 单元测试

### 知识点

##### CSS

- `:not` 伪类
- `:first-child` 伪类
- `:last-child` 伪类

##### JavaScript

- `Element.children` 是一个只读属性，只返回当前元素所包含元素节点的实时的 HTMLCollection。
- `Element.classList` 是一个只读属性，返回一个元素的类属性的实时 DOMTokenList 集合，该属性还有 `add`、`remove`、`toggle`、`contains` 方法。

##### Vue

- 父子组件的 `created`、`mounted` 顺序？

  1. 父组件先 created。
  2. 子组件接着 created。
  3. 子组件接着 mounted。
  4. 父组件最终 mounted。

- `vm.$children` 获取当前实例的直接子组件，就算子组件被 HTML 元素包裹也算是直接子组件：

  ```vue
  <l-button-group>
    <div>
      <l-button icon="left">上一页</l-button>
    </div>
    <l-button icon="right" icon-position="right">下一页</l-button>
  </l-button-group>
  ```

  上述代码中，ButtonGroup 仍然含有两个直接子组件：Button。

- `vm.$el` 获取当前实例使用的根元素。