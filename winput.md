# WInput

### 基本流程

- 需求分析
  - 用户操作分析：鼠标点击后键盘输入、可复制粘贴、可使用 Tab 键选中
  - `WInput` 组件状态：
    - 鼠标 hover、active、focus
    - 组件自身不可用状态、只读状态
    - 配合表单提交的提示：成功状态、错误状态
-  UI 设计
- 代码开发
  - 用户怎么使用
  - `props`：`type`、`value`、`disabled`、`readonly`、`autocomplete`、`placeholder`、`maxlength`、`minlength`、`autofocus`、`tabindex`、`name`、`clearable`
- 单元测试

### 知识点

- `<input>` 元素学习，包括它有哪些 `type` 、`attributes`
- CSS 属性选择器 => 整个 CSS 选择器的使用
- Vue 组件中 `name` 的作用
- Vue  `<style>` 元素 `scoped` 属性的作用
- CSS `cursor` 属性有哪些设置光标的类型
- CSS `box-shadow` 的用法
- Vue 使用 `$emit` 监听子组件事件，`$emit` 的参数使用
- Vue 组件实现 `v-model`
- `change`、`input`、`focus`、`blur`事件的不同
