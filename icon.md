# Icon 组件

### 基本流程

图标作为一种符号可以减少用户的认知成本，还可以增强页面的可读性、突出重点。WIcon 是一个语义化的矢量图标组件。

##### 需求分析

- 可以设置不同图标
- 可以设置颜色
- 可以设置大小
- 可以触发 `click` 事件

##### UI 设计

##### 代码开发

- 用户怎么使用

  ```vue
  <l-icon name="setting" />
  <l-icon name="plus" color="red" />
  <l-icon name="plus" :size="32" />
  ```

- `props`

  - `name`

  - `color`
  - `size`

- `event`

  - `click`

##### 单元测试

使用 `vue-test-utils` 和 `Jest` 进行单元测试。

