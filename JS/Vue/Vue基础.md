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

- `v-html`：输出变量数据到html标签中，若内容含html代码可被浏览器转换。

- `v-text`：输出变量数据到html标签中，若内容含html代码则会转换成实体字符，不会被浏览器转换。

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

### Vue实例常用属性

- 生命周期内各时间点触发的对象属性
  - `beforeCreate:Function`：在Vue实例创建完成之前触发beforeCreate`属性的匿名函数。
  - `created:Function`：在Vue实例创建完成之后触发`created`属性的匿名函数。
  - `beforeMount:Function`：在Vue实例挂在到html实体前触发`beforeMount`属性的匿名函数。
  - `mounted:Function`：在Vue实例挂在到html实体后触发`mounted`属性的匿名函数。
  - `beforeDestory:Function`：在Vue实例销毁前触发`beforeDestory`属性的匿名函数。
  - `destoryed:Function`：在Vue实例销毁后触发`destoryed`属性的匿名函数，Vue实例的所有属性方法都不能在此方法内使用。

- Vue实例运行中常用的对象属性

  - `methods:object`：在Vue实例中的methods的json对象中可定义多个方法调用。

  - `computed:object`：计算属性，在object中可定义多个方法，方法中涉及到的数据发生变更的时候都会执行对应的方法，如：

    ```html
    <div id="app">{{fullName}}</div>
    
    <script>
        var app = new Vue({
           el:"#app",
           data:{
               firstName: "Nelg",
               lastName: "Wang"
            },
            computed:{
                //简易，Vue实例中的data对应的属性值方法变化的时候执行
                fullName: function() {
                    return this.firstName + " " + this.lastName;
                },
                //复杂
                fullName: {
                    //Vue实例中的data对应的属性值方法变化的时候执行
                    get: function() {
                    	return this.firstName + " " + this.lastName;
                	},
                    //通过app.$fullName = 'Nelg wang'设置fullName的值的时候触发set方法，设置的值通过形参value传入。
                    set: function(value) {
                        let arr = value.splice(" ");
                        this.firstName = arr[0];
                        this.lastName = arr[1];
                    }
                }
            }
        });
    </script>
    ```

    