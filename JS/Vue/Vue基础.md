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
           props:{
               //单类型
               content: String,
               //多类型
               content: [String, Number, Object],
               //复杂用法
               content: {
                   type: [String, Number, Object],
                   //是否必传
                   required: true，
                   //默认值
                   default: '123',
                   //验证
                   validator: function(value) {
                   	return (value.length > 5);
               	}
               }
           },//传递父级组件数据到子组件中，这里是将item传递到子组件的content中。
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
           props:{
               //单类型
               content: String,
               //多类型
               content: [String, Number, Object],
               //复杂用法
               content: {
                   type: [String, Number, Object],
                   //是否必传
                   required: true，
                   //默认值
                   default: '123'
               }
           },//传递父级组件数据到子组件中，这里是将item传递到子组件的content中。
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
                        //因为fullName的get方法中涉及到firstName和lastName属性，
                        //因此当set方法执行完之后还会执行get方法。
                    }
                }
            }
        });
    </script>
    ```

### Vue注意项

- 数组
  - 当直接改变数组某个元素是是不会执行页面数据刷新的。因为在Vue实例中data字段中的每个元素都是一个拓展的Vue元素属性，只能使用Vue提供的7个方法对里面的元素进行修改，7个方法分别是：
    - pop：在数组的尾部删除一个元素
    - push：在数组的尾部添加一个元素
    - shift：在数组的头部删除一个元素
    - unshift：在数组的头部添加一个元素
    - splice：在数组的指定位置开始删除指定个数的元素，第三个参数是在数组指定的位置添加一个元素
    - sort：对数组进行排序
    - reverse：对数组进行反转

- 页面元素的挂载

  - 在挂载页面元素的时候，可能会出现以下情况：

    ```html
    <table>
    	<tbody>
        	<row></row>
        	<row></row>
        	<row></row>
        </tbody>
    </table>
    
    <script>
        Vue.component('row', {
            template: "<tr><td>This is row</td></tr>"
        });
        var app = new Vue({
           el:"#app"
        });
    </script>
    ```

    > 当页面运行的时候有可能`row`子组件并没有挂载在tbody中，这是因为h5的标签规则规定了table标签中一定要有tbody，tbody下只能嵌入tr标签，当有其他类型的html标签嵌入的时候就会将标签转移到tbody之外，因此Vue给出的解决方案是使用`:is="组件名"`的标签语法，如下：

    ```html
    <table>
    	<tbody>
        	<tr :is="row"></tr>
        	<tr :is="row"></tr>
        	<tr :is="row"></tr>
        </tbody>
    </table>
    
    <script>
    	Vue.component('row', {
            template: "<tr><td>This is row</td></tr>"
        });
        var app = new Vue({
           el:"#app"
        });
    </script>
    ```

  - 在Vue中不建议使用id属性绑定元素，而是使用`key="唯一标识"`的方法来绑定页面元素。

- 事件监听：

  - 在Vue中，若是在子组件挂载的html节点挂载点绑定事件表示是监听自定义事件，可以通过vm.$emit("事件名")，触发父级组件监听的自定义事件，子组件的事件需要在挂载元素的实体html中定义。若需要在子组件的挂载点需要定义原生事件需要在事件名后添加.native属性。如：@click.native。

### Template标签

`template`html标签在Vue中只作占位标签作用，因此template不会对现有的html以及css样式产生影响。