# Message 组件

Message 组件用于展示操作反馈的信息，它在用户进行操作后显示在顶部居中位置并可自动消失。

### 基本流程

1. 需求分析

   用户操作后弹出提示信息

   - 提示信息会一段时间后自动消失
   - 提示信息有各种类型：普通、成功、警告、失败等
   - 提示信息可以有关闭按钮，并可设置回调
   - 提示信息可以是有多个

2. UI 设计

3. 代码开发

   - 用户怎么使用：

     ```vue
     <template>
       <button @click="showMessage">展示提示信息</button>
     </template>
     
     <script>
     export default {
       // ...
       methods: {
         showMessage () {
           this.$message({
             content: '当前操作有误',
             duration: 5
           })
         }
       }
     }
     </script>
     ```

   - Messge 组件使用的方式是在 Vue 实例上调用，要实现这种功能，可以通过开发 Vue 插件的方式来实现

     - 使用插件的方式：

       ```javascript
       // 在项目的入口文件中引入对应插件，然后使用 Vue.use()
       import message from './plugin/message.js'
       
       Vue.use(message)
       ```

     - 开发插件，Vue 插件应该暴露一个 `install` 的方法，该方法的第一个参数是 Vue 构造器，第二个参数是可选的参数对象：

       ```javascript
       export default {
         install (Vue, options) {
           Vue.prototype.$message = () => {
             //
           }
         }
       }
       ```

   - 怎么在插件中使用 Message 组件？使用动态创建组件的方式：

     ```javascript
     import WMessage from '../components/WMessage.vue'
     
     export default {
       install (Vue, options) {
         Vue.prototype.$message = () => {
           const Constructor = Vue.extend(WMessage)
           const message = new Constructor()
           // ...
           message.$mount()
           document.body.appendChild(message.$el)
         }
       }
     }
     ```

   - 页面中出现多个 Message 并且有间隔地竖向排列的思路：

     1. 先获取第一个 Message 距离页面顶部的距离

        ```javascript
        let topReal = messageInstance.top
        ```

     2. 遍历所有的 Message 拿到其高度并加上一个间隔值（表示两个 Message 之间的间隔）然后使用 `+=` 赋值给 `topReal`：

        ```javascript
        messageInstances.forEach(item => {
          topReal += item.$el.offsetHeight + 16
        })
        ```

     3. 将 `topReal` 赋值给 Message top：

        ```javascript
        messageInstance.top = topReal
        ```

     4. 将 Message 放进一个表示所有 Message 的数组中：

        ```javascript
        messageInstances.push(messageInstance)
        ```

   - 页面中存在多个 Message 时，通过 Message.close 方法来关闭：

     - 通过 id 找到要关闭的 Message
     - 调整被关闭 Message 的后面 Message 的 top

   - 调试技巧：在页面中凡是看到有宽高存在的元素，但是使用 JS API 获取的宽高不存在时，一般是元素还未出现在页面里，可以使用 `$nextTick()` 来解决。

   - props：`content`、`type`、`icon`、`duration`、`top`、`closeable`、`onClose`、

     `iconColor`、`dangerouslyEnableHTML`

4. 单元测试

### 知识点

##### CSS

- `min-height` 不是真实高度，父元素设置 `min-height: 100px`，子元素设置 `height: 100%`，此时子元素的真实高度不存在 

##### JavaScript

- getBoundingRect()

##### Vue

- 插件
- 动画