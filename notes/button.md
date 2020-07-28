# button

### 基本流程

- 按钮组件需求分析：使用用例图分析
  
  - 基本状体分析：可点击状态、禁用状态、loading 状态、hover 状态、按下状态
  
- UI 设计

- 代码开发

  - 用户怎么使用

    ```html
    <template>
    	<w-button>默认按钮</w-button>
      <w-button type="primary">主要按钮</w-button>
      <w-button type="primary" shape="round">圆角按钮</w-button>
      <w-button type="primary" shape="circle">圆形按钮</w-button>
      <w-button icon="upload">带图标的按钮</w-button>
      <w-button :loading="loading">加载中</w-button>
      <w-button disabled>不可用状态</w-button>
    </template>
    ```

  - 按钮 `props`：`type`、`icon`、`shape`、`loading`、`disabled`

- 单元测试

### 知识点

- `Vue.component()` 使用
- `:root` 伪类
- CSS 变量
- `var()` 函数
- `:link`、`:visitied`、`:hover`、`:focus`、`:active` 伪类
- `npm install -S` 和 `npm install -D` 区别
- Vue 不同版本
- Vue 单文件组件及其命名风格
- `npx` 命令
- Vue Class 绑定
- Prop
- Iconfont 的使用
- CSS 属性选择器
- HTML disabled 属性
- CSS 动画使用
