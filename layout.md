# Layout 布局

### 基本流程

##### 需求分析

- 组件定义

  Layout 是页面基础布局框架，常与其它组件嵌套使用，构成页面整体布局。

- 组件构成

  - LLayout：布局容器，可以放在任何父容器中，LLayout、LHeader、LSider、LContent、LFooter 都可以作为其子组件。
  - LHeader：顶部导航，自带默认样式，可嵌套任意组件和元素，只能放在 LLayout 中，其高度范围计算公式：48 + 8n。
  - LSider：侧边栏，自带默认样式，可嵌套任意组件和元素，只能放在 LLayout 中，其宽度范围计算公式：200 + 8n。
  - LContent：内容部分，自带默认样式，可嵌套任意组件和元素，只能放在 LLayout 中。
  - LFooter：页面底部，自带默认样式，可嵌套任意组件和元素，只能放在 LLayout 中。

##### UI 设计

常见的基本布局如下：

![layout-1](./imgs/layout-1.png)

![layout-2](./imgs/layout-2.png)

![layout-3](./imgs/layout-3.png)

![layout-4](./imgs/layout-4.png)

##### 代码开发

- 用户怎么使用：

  ```vue
  <template>
    <div>
      <l-layout>
        <l-header>header</l-header>
        <l-content>content</l-content>
        <l-footer>footer</l-footer>
      </l-layout>
      
      <l-layout>
        <l-header>header</l-header>
        <l-layout>
          <l-sider>sider</w-sider>
          <l-content>content</w-content>
        </l-layout>
        <l-footer>footer</w-footer>
      </l-layout>
      
      <l-layout>
        <l-header>header</l-header>
        <l-layout>
          <l-content>content</l-content>
          <l-sider>sider</l-sider>
        </l-layout>
        <l-footer>footer</l-footer>
      </l-layout>
      
      <l-layout>
        <l-sider>sider</l-sider>
        <l-layout>
          <l-header>header</l-header>
          <l-content>content</l-content>
          <l-footer>footer</l-footer>
        </l-layout>
      </l-layout>
    </div>
  </template>
  ```

- LSider 组件 props：
  - width：Sider 的宽度，默认是 `200px`。
  - theme：Sider 的主题色。
  - collapsible：Sider 是否可以收起，默认值是 `false`，表示 Sider 是展开的。
  - hideTrigger：是否隐藏 Sider 底部默认的触发器，默认隐藏。
  - collapsed：Sider 当前是收起还是展开，需要使用 `v-model`。
  - defaultCollapsed：Sider 默认收起/展开的状态。
  - collapsedWidth：Sider 收起时的宽度。
  - reverseArrow：翻转底部触发器中提示箭头的方向，当 Sider 在右边的时候可用。
  - breakpoint：响应式布局的短点，当页面宽度小于断点值时，Sider 收起；当页面宽度大于断点值时，Sider 展开。
  
- LSider 组件 events：
  
  - collapse，参数如下：
    - collapsed，布尔值。
    - type，字符串值，可用值为：clickTrigger | responsiveTrigger。
  - breakpoint，参数为：collapsed，布尔值。
  
- LSider 实现收起/展开的思路：
  
  1. 收起/展开的前提：
  
     - Sider 只有在 `collapsible: true` 和 `hideTrigger: false` 的情况下才能收起/展开。
  
     - 收缩模块的 `background-color` 和 `color` 要根据 Sider 组件的 theme prop 确定。
  
     - 收缩模块处于底部的方法：
  
       ```html
       <aside>
         <div>默认插槽</div>
         <div>收缩模块</div>
       </aside>
       ```
  
       当 `aside` 元素设置 `padding-bottom: 48px`，“默认插槽”部分设置 `height: 100%`，“收缩模块”设置 `line-height: 48px`，这样就刚好将收缩模块置于底部，并且留下了收缩模块的高度空间。
  
  2. 收起/展开的状态：
  
     1. 在 data 中添加 property `collapseLocal`
  
        - 初始值：由 collapsed 和 defaultCollapsed 决定，判断逻辑为：当 collapsed 不是 `undefined` 时，Sider 的收起/展开状态由 collapsed 控制；否则就由 defaultCollapsed 决定。
  
        - 当收缩模块触发点击事件时，对 `collapsedLocal` 的值进行取反，并将变化后的值通过 `$emit('update:collapsed', value)` 的方式传递到父组件上。
  
          ```javascript
          {
            methods: {
              onClickTrigger () {
                // 对当前收起/展开状态取反
                this.collapsedLocal = !this.collapsedLocal
                // 将改变后的值传递给父组件
                this.$emit('update:collapsed', this.collapsedLocal)
              }
            }
          }
          ```
  
     2. collapsed 支持 `v-model` 指令，使用 `model` 选项修改默认的 prop 和 event：
  
        ```js
        {
          model: {
            prop: 'collapsed',
            event: 'update:collapsed'
          }
        }
        ```
  
  3. 收起/展开的宽度：
  
     添加计算属性 `styleWidth`
  
     - 当 `collapsedLocal` 为 true 时，Sider 宽度为 `collapsedWidth` 的值。
     - 当 `collapsedLocal` 为 false 时，Sider 宽度为 `width` 的值。
  
  4. 收起/展开的效果：
  
     给 `.l-layout-sider` 类设置 `transition` 属性。

##### 单元测试

### 知识点

##### CSS

- `overflow`
- `transition`

##### JavaScript

- `matchMedia()`

- `MediaQueryList` 是一个存储文档媒体查询信息的对象，它可以通过 `window.matchMedia()` 方法创建。生成的对象在媒体查询状态变更时（例如：媒体查询开始或者停止计算是为 true）可以向侦听器发送通知。示例如下：

  ```javascript
  const mql = window.matchMedia('(max-width: 1920px)')
  console.log(mql)
  /*
  {
    matches: true,
    media: '(max-width: 1920px)',
    onchange: null
  }
  */
  ```

  - 属性
    - `matches`，只读的布尔属性，用以表示文档与当前媒体查询列表是否匹配。其中，true 表示匹配；false 表示不匹配。
    - `media`，只读，表示媒体查询的字符串。
  - 事件
    - `change`：只有在对文档进行媒体查询的结果发生改变时触发。

- `in`

##### Vue

- Vue 动画
- `v-model`
- `model` 选项用于修改 `v-modle` 指令默认使用的 prop 和 event
