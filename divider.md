# Divider 组件

### 基本流程

##### 需求分析

- 组件定义

  分割线组件用于划分内容区域，对模块做隔离。

- 何时使用

  当需要进行内容分割时。

##### UI 设计

##### 代码开发

- 用户怎么使用

  ```vue
  <l-divider />
  <l-divider color="#eee" />
  <l-divider type="dashed" />
  <l-divider direction="vertical" />
  <l-divider>文本</l-divider>
  <l-divider text-position="right">文本</l-divider>
  ```

- props

  - type：分割线类型。
  - color：分割线颜色。
  - gap：水平方向时，gap 是上下间距；垂直方向时，分割线是左右间距。
  - direction：分割线方向。默认是水平方向，当设置垂直方向时分隔线组件不能带文字。
  - text-position：文字在水平方向的位置分布。

##### 单元测试

### 知识点

##### CSS

- `line-height`
- `vertical-align`
- `border-style`