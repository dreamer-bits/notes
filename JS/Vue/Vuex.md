# Vuex使用

### Vuex介绍

​	Vuex是Vue官方提供的数据框架，用于组件之间的数据传递。下图为Vuex的使用流程：

![](./images/vuex.png)

### 安装Vuex

`npm i vuex --save`

### 使用Vuex

1. 在`/src`目录下创建`store`数据仓库目录，再创建`index.js`文件

2. 编写`index.js`文件：

   ```javascript
   //引入Vue原形Class用于注册Vuex
   import Vue from 'vue';
   import Vuex from 'vuex';
   
   //初始化Vuex
   Vue.use(Vuex);
   
   //初始化Vuex并返回
   export default new Vuex.Store({
     //state字段用于存放数据
     state: {
       city: '广州'
     },
     //actions中定义的各种方法是用于接受需要修改的state的数据
     actions: {
       changeCityActions(ctx, city) {
          //ctx变量是通过commit用来触发mutations中的方法的
          ctx.commit("changeCity", city);
       }
     },
     mutations: {
       changeCity(state, city) {
         //state即Vuex中存放数据的state字段
         state.city = city;
       }
     }
   });
   ```

3. 引入Vuex到项目中：

   > 在`/src/main.js`文件中添加以下代码：

   ```javascript
   //引入刚刚初始化的Vuex对象
   import store from './store';
   
   new Vue({
     el: '#app',
     router,
     //在Vue中添加store属性，并注入Vuex对象
     store,
     components: { App },
     template: '<App/>'
   });
   ```

4. Vuex的使用流程

   1. 完整流程：
      1. 在Vue各组件中调用`this.$store.dispatch(actions中的方法, 传值);`即可触发Vuex中的`actions`字段中的指定方法。
      2. 在`actions`字段中的方法通过`ctx.commit(mutations中的方法, 传值);`即可将需要修改的数据传入到`mutations`字段中触发的指定方法中。
      3. 在`mutations`字段中的方法通过`state`变量获得Vuex中`state`字段的数据，并进行修改。
   2. 快捷流程：
      1. 在Vue各组件中直接调用`this.$store.commit('changeCity', city);`即可直接触发`mutations`字段中指定方法并传值。
      2. 在`mutations`字段中的方法通过`state`变量获得Vuex中`state`字段的数据，并进行修改。