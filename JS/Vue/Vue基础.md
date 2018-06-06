# Vue基础语法

### 生命周期

![](./images/lifecycle.png)

### 基础指令

- `v-on`：绑定/监听事件，简写方式`@事件`。此事件可为常规事件，如click、change或子组件自定义事件。

- `v-model`：数据传输。主要用于表单类型的数据传输，通过页面操作或用户输入，将用户输入值绑定到一个变量上，保存在Vue实例中。如：

  ```html
  <div id="app">
  	<input v-model="{{test}}"/>
  </div>

  <script>
      var app = new Vue({
          el:"#app",
          data:{
              test:''
          }
      });
  </script>
  ```

- `v-for`：数据遍历。在页面元素上绑定此指令，即可根据数据的条数循环创建元素。如：

  ```html
  <li v-for="(item,index) in array">
  ```

- `v-bind`：绑定数据，可简写为`:数据/html特殊属性`，如：

  ```html
  <div :class="[属性1，属性2]" :style="{dispaly:none}"></div>
  ```

### 创建子组件

1. 创建全局子组件：

   ```html
   <ul id="app">
       <Item :content="item" v-for:"item in array"></item>
   </ul>

   <script>
       Vue.component("Item", {
           props:['content'],		//传递父级组件数据到子组件中，这里是将item传递到子组件的content中。
           template: "<li>{{content}}</li>"
       });
       var app = new Vue({
          el:"#app",
          data:{
              list:[]
           }
       });
   </script>
   ```

2. 创建Vue实例的专属子组件：

   ```html
   <ul id="app">
       <Item :content="item" v-for:"item in array"></item>
   </ul>

   <script>
       var Item = {
           props:['content'],		//传递父级组件数据到子组件中，这里是将item传递到子组件的content中。
           template: "<li>{{content}}</li>"
       });
       var app = new Vue({
          el:"#app",
          components:{
              Item:Item
          },
          data:{
              list:[]
           }
       });
   </script>
   ```

3. 子组件往父组件传值：

   ```html
   <ul id="app">
       <Item
       	:content="item" 
           v-for:"item in array"
           @delte="pullValue"
       ></item>
   </ul>

   <script>
       var Item = {
           props:['content'],		//传递父级组件数据到子组件中，这里是将item传递到子组件的content中。
           template: "<li @click='pushValue(content)'>{{content}}</li>",
           methods:{
               pushValue: function(value) {
                   this.$emit("delete", value);	//触发当前实例（即根实例）上的事件
               }
           }
       });
       var app = new Vue({
          el:"#app",
          components:{
              Item:Item
          },
          data:{
              list:[]
           },
           methods:{
               pullValue:function(value) {
                   console.log(value);
               }
           }
       });
   </script>
   ```

   ​