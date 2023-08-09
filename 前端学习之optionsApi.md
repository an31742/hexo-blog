---
title: 前端学习之optionsApi
date: 2022-07-07 19:13:24
tags: vue3
---
# computed计算属性

1.将逻辑抽取到methods，放到methods中的options中

**不足：所有的data使用过程都变成了方法的调用**

2.使用计算属性

* 定义：可以包含任何响应式的复杂逻辑
* 适用范围：计算属性将被混入组件实例中，所有getter和setter this上下文自动绑定为组件实例

3.计算属性与其他对比的优点

| 例子                                                                                                                                                     | 模板语法                                                                                                       | methods的实现                                                               | computed实现         |
| -------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------- |
| firstName和lastName，希望它们拼接之后在界面上显示<br />一个分数大于60显示几个，小于60显示不及格<br />message变量，在情况确定显示直接显示文字，不确定反转 | 不足：存在大量逻辑不便于维护<br />有多次一样的逻辑存在重复代码<br />多次使用的时候多运算也需要多次执行没有缓存 | 不足：都是先显示一个结果变成了方法的调用<br/>多次使用没有缓存也需要多次计算 | 解决前两个的问题不足 |

4.methods和computed的区别：

* computed有缓存不需要多次计算  计算属性的缓存只有当数据发生变化的时候计算属性会重新计算性能提升，methods每次调用都会重新计算一次computed不会

5.计算属性的方法

* getter :与methods方法类似 返回一个值
* setter:获取到一个value值，使用这个value操作

6.源码对setter和getter的处理

* 只对这个做了一个判断

7.vue3不支持使用过滤器可以使用计算属性



# watch侦听器

1. 适用范围：监听某个数据的变化

2. 监听的范围：默认情况只监听数据本身的变化

3. 使用方法：

   1. ```js
      //对象的监听方法
      watch:{
          info:{
             handler(newVale,oldValue){ 
               console.log(newValue,oldValue)
            }
            deep:true, //深度监听
            immediate:true, //立即执行
            'info.name':function(newVale,oldValue){
                 console.log(newValue,oldValue)
            }
        }
      }
      ```

   2. ```
      //第一个参数使侦听的源
      //第二个是回调函数
      //第三个是额外的其它选项
      created(){
           this.$watch('meaasge',(newValue,oldValue)=>{
           console.log(newValue,oldValue)
       },deep:true,immediate:true) 
      }
      
      
      ```


   3.不会深度直接监听一个数组，将数组转换成组件，将需要监听的属性传过去，在子组件监听传过去的属性

# v-model响应式

1. 业务范围：登录注册需要提交账户和密码

2. 表单控件范围：input  textarea select

3. 本质原理：v-bind绑定value值  v-on绑定input事件监听到函数中 函数会获取最新的赋值绑定到属性中

4. 绑定控件
   1. 绑定checkbox  
      1.  单个勾选框   v-model是布尔值 input的value值并不影响v-model的值
      2. 多个勾选框  data对应的是一个数组  就会将input的value添加到数组中
   2. 绑定radio  用于选择其中的一项
   3. 绑定select  
      1. 绑定一个值会将他对应的值付给v-model对应data
      2. 绑定的是一个数组就会将选中option对应的value值付给v-model对应的data

5. v-model的修饰符
   1. lazy   会将绑定的事件切换为change事件只有在提交的时候才触发
   2. number 会将字符串其他类型转换成数字的数据类型    因为数字类型会自动转换成字符串
   3. trim 自动过滤输入的空白字符
   4. v-model也可以在组件上使用

6. **补充知识**

   1. ```js
      //浅拷贝
      const info ={name:"kobe",age:18,friend:{name:"jams"}}
      const obj=Object.assign({},info)
      
      info.frined.name="礼拜"
      console.log(obj.friend.name)  //礼拜
      
      ```

   2. 逻辑判断时候会自动将字符串转换成number类型

# 组件式开发

1.组件的定义：项目由独立且可复用的组件构成

2.组件的注册的方式：

1. 全局组件   都可以使用    

   ```js
         const app = Vue.createApp(App);
   
         // 使用app注册一个全局组件app.component()
         // 全局组件: 意味着注册的这个组件可以在任何的组件模板中使用
         app.component("component-a", {
           template: "#component-a",
           data() {
             return {
               title: "我是标题",
               desc: "我是内容, 哈哈哈哈哈",
             };
           },
           methods: {
             btnClick() {
               console.log("按钮的点击");
             },
           },
         });
   ```

   

2. 局部组件  只有局部可以使用

   在app组件中还有一个components对象，对象中的键值对是 组件的名称: 组件对象 。引入外部文件就可以使用了

3.组件名称： 

1. kebab-case (短横线分隔命名) 定义一个组件时，你也必须在引用这个自定义元素时使用 kebab-case， 
2. 当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。短横线和驼峰都是可以接受的
3. 当在使用的时候最好用名字加横线的方式


  


        

    

    

    

    

    

    

    

    

    

     

      

    ​    

    ​    

    ​    

    ​    

    ​    

    ​    

    ​    

    ​    

    ​    

    ​    

    ​    



