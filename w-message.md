# Message 组件

### 基本流程

##### 需求分析

Message 组件用于展示全局操作反馈的信息。用户进行操作后会显示在顶部居中位置：

- 提示信息在页面上显示一段时间后会自动关闭，可配置在页面上的持续时间。
- 提示信息可以手动关闭，需要显示关闭按钮，并可添加回调。
- 提示信息可以出现多个。
- 提示信息有各种类型：info、success、error、warning。

##### UI 设计

##### 代码开发

- 用户怎么使用：

  ```vue
  <template>
    <button @click="handleConfirm">确定</button>
  </template>
  
  <script>
  export default {
    // ...
    methods: {
      handleConfirm () {
        this.$message('验证失败')
      },
      handleSuccess () {
        this.$message({
          type: 'success',
          content: '修改信息成功'
        })
      },
      handleError () {
  			this.$message({
          type: 'error',
          content: '提交失败'
        })
      }
    }
  }
  </script>
  ```

- 用户要想像上面这样使用，可以通过给 `Vue.prototype` 添加一个全局的方法 `$message` 来实现。为此，需要开发 Vue 插件。

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

  - 使用插件：

    ```javascript
    // 在项目的入口文件中引入对应插件，然后使用 Vue.use()
    import message from './plugin/message.js'
    
    Vue.use(message)
    ```

- 在开发 Vue 插件时，需要使用 Message 组件，以下是动态创建实例的方式：

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

- WMessage 组件的 UI 由三部分组成，分别是：前缀图标、消息体、关闭图标。

- WMessage 组件的 props：

  - `type`
  - `icon`
  - `iconColor`
  - `content`
  - `dangerouslyUseHTML`
  - `duration`
  - `closeable`
  - `onClose`
  - `top`
  
- WMessage 组件通过 Vue 自有组件 `<transition>` 添加过渡效果，在使用中有几个问题：

  - 过渡的触发条件：动态组件、组件根节点是什么意思。
  - CSS `transition: translate(-50%, -100%)` => `transition: translate(-50%, 0)` 在过渡时无效问题。
  
- WMessage 组件实现页面中出现多个 WMessage 且有间隔地竖向排列的效果，此时假设 WMessage 不会关闭，思路如下：

  1. 先获取第一个 WMessage 距离页面距离页面顶部的距离：

     ```javascript
     let topValue = message.top
     ```

  2. 遍历 `messageList` 中的所有 WMessage 实例，拿到每个实例的 `offsetHeight` 并加上间隔值：

     ```javascript
     messageList.forEach(item => {
       const { offsetHeight } = item.$el
       topValue += offsetHeight + baseGap
     })
     ```

  3. 将遍历后得到的 `topValue` 再赋值给当前新创建的 WMessage 实例：

     ```javascript
     message.top = topValue
     ```

  4. 最后将这个新创建的 WMessage 实例放入 `messageList` 中：

     ```javascript
     messageList.push(message)
     ```

- 考虑 WMessage 组件会自动关闭的情况：

  1. 首先在 WMessage 组件自动关闭时要 `$emit` close 事件，这样便于在实例中监听。

     ```javascript
     // WMessage.vue
     close() {
       // ...
       this.$emit('close', this)
     }
     ```

  2. 然后在创建的 WMessage 实例中监听 close 事件。

     ```javascript
     // message.js
     message.$on('close', (currentMessage) => {
       // ...
     })
     ```

  3. 需要知道是哪个 WMessage 被关闭。

     - 为此，需要给每个实例添加一个 id

       ```javascript
       message.id = `w-message-${count++}`
       ```

     - 遍历 `messageList`，如果有和当前关闭实例的 id 相等的项，则将它从 `messageList` 中删除，并记录它自身的高度和在 `messageList` 中 index。

       ```javascript
       message.$on('close', (currentMessage) => {
         let index = -1
         let heightRemoved = 0
         // 找到被关闭的实例
         messageList.forEach((item, i) => {
           if (item.id === currentMessage.id) {
             index = i
             messageList.splice(i, 1)
             heightRemoved = currentMessage.$el.offsetHeight
           }
         })
       })
       ```

     - 还须修改将在该项 index 之后的所有实例的 `top` 值。

       ```javascript
       message.$on('close', (currentMessage) => {
         let index = -1
         let heightRemoved = 0
         messageList.forEach((item, i) => {
           if (item.id === currentMessage.id) {
             index = i
             messageList.splice(i, 1)
             heightRemoved = currentMessage.$el.offsetHeight
           }
         })
         // 这两种情况不用在做后续操作
         if (length < 1 || index > length - 1) {
           return
         }
         // 被关闭的后面的所有实例都要修改 top
         const length = messageList.length
         for (let i = index; i < length; i++) {
           const messageElement = messageList[i].$el
           const topValue = parseInt(messageElement.style.top, 10)
           const topShould = topValue - heightRemoved - gapBase
           messageElement.style.top = topShould + 'px'
         }
       })
       ```

     - 在 WMessage 组件中设置 top 变化的 transition 效果。

       ```css
       .w-message-slide-enter-active {
         transition: all .3s;
       }
       .w-message-slide-leave-active {
         transition: all .2s;
       }
       .w-message-wrapper {
         // ...
         transition: all .3s;
       }
       ```

##### 单元测试


### 知识点

##### CSS

- `min-height` 不是真实高度，父元素设置 `min-height: 100px`，子元素设置 `height: 100%`，此时子元素的真实高度不存在。
- `pointer-events`

##### JavaScript

- `getBoundingRect()`
- `offsetHeight`

##### Vue

- 插件开发
- `<transition>`