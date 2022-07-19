---
title: 前端学习之vuecli项目搭建
date: 2022-07-19 18:10:12
tags: vue3
---

# vue 项目创建
1.cli 脚手架
   1. 英语：Command-Line Interface
   2. 英语：Command-Line Interface
   3. 作用：选择项目的配置和创建项目  内部已经配置了webpack
   4. 安装:npm install @vue/cli -g   vue create 项目的名称 创建项目  
   5. 创建项目各个依赖配置  要掌握
   6. 原生浏览器支持ES module但是有很多问题
   7. 创建项目 
      1. vue create 项目的名称
      2. Your connection to the default yarn registry seems to be slow.     
         Use https://registry.npmmirror.com for faster installation   是否使用淘宝镜像选择Y
      3. ``` js
          Default ([Vue 3] babel, eslint)    //默认vue3项目
          Default ([Vue 2] babel, eslint)   //默认vue2项目
          Manually select features     //手动选择
         ```
      4. ``` js
          (*) Babel  //es6 转换es5  
          ( ) TypeScript  //ts
          ( ) Progressive Web App (PWA) Support
          ( ) Router  //路由
          ( ) Vuex    //Vuex
          ( ) CSS Pre-processors  //css预处理器
          (*) Linter / Formatter  //格式化代码
          ( ) Unit Testing  //单元测试
          ( ) E2E Testing  //E2E测试
         ```
      5. ```
         Vue CLI v5.0.8
         ? Please pick a preset: Manually select features
         ? Check the features needed for your project: Babel
         ? Choose a version of Vue.js that you want to start the project with 3.x      
         ? Where do you prefer placing config for Babel, ESLint, etc.? (Use arrow keys)
         > In dedicated config files   是否设置单独的还是都放在package.json里面 选择单独的
         In package.json
         ```
      6. ? In dedicated config files ? Save this as a preset for future projects  //是否保存刚才已经选择过的下次就不用再选了
   
2.项目目录

3vuecli 运行原理
   1. 在vue-cli-service是一个软连接真正的在cli-service里面
   2. node_modules里面有一个@vue文件夹有一个cli-service这里面真正的代码

4认识vite 
   1. webpack是目前使用最多的架构工具
   2. 优点：快速 提升开发体验  对node_module预打包提高速度
   3. 组成： 
      1. 基于原生ES模块提供了丰富的内建功能  HMR非常快
      2. 是预配置的可生成环境优化过的静态资源
   4. 安装 npm install vite –g # 全局安装 npm install vite –D # 局部安装
   5. npx vite来启动项目  搭建一个服务
   6. 支持依赖:
      1. css直接导入
      2. less直接导入 安装npm install less -D
      3. 直接支持postcss  npm install postcss postcss-preset-env -D  配置postcss.config.js
      4. typeScript 原生支持
      5. 对vue第一优先级支持 npm install @vitejs/plugin-vue -D 安装vue插件
      6. 在vite.config.js配置插件
   7. **原理**:
      1. vite搭建一个本地服务器
         1.   vite 创建本地服务器vite1用搭建本地服务器koa  
         2.   vite2用的是搭建本地服务器Connet  
         3.   webpack创建本地服务器express
      2. 将ts less 等文件转换成js中的ES6的代码
      3. 使用connect将请求做并且做一个转发

   8. vite对vue是直接的支持
      1. Vue 3 单文件组件支持：@vitejs/plugin-vue
      2. Vue 3 JSX 支持：@vitejs/plugin-vue-jsx
      3. Vue 2 支持：underfin/vite-plugin-vue2
      4. 在vite.config.js配置插件
         1. ```js
           import vue form "@vitejs/plugin-vue"
           module.exports={
            plugins:{
               vue()
            }
           }
            ```
   9.  npx vite build 打包  
   10. npx vite preview 开启本地服务预览打包  打包使用esbuild
  

5.ESBuild解析
   1. 优点：
      1. 超快的构建速度，并且不需要缓存；
      2. 支持ES6和CommonJS的模块化；
      3. 支持ES6的Tree Shaking；
      4. 支持Go、JavaScript的API
      5. 支持TypeScript、JSX等语法编译
      6. 支持SourceMap；
      7. 支持代码压缩；
      8. 支持扩展其他插件
   2. 原理：
      1. 使用GO语言直接转成机器码转成机器码
      2. 充分利用cpu的多核预运行
      3. 从零编写不使用第三方，优化性能
   

6.vit构建脚手架工具
   1. 安装 npm install @vitejs/create-app -g  create-app 项目名字

# vue3组件化开发
1. 将组件进行拆分
   1. App分为三个组件 Header Main Footer
   2. main 分为 banner 和 ProducList   
   3. 组件中css作用域父组件样式穿透需要子组件增加一个跟元素

2. 组件之间的通信
   1. 父组件->子组件  props
      1. props 有两种方式
         1. 字符串数组  ['title',"content"]
         2. 对象类型  {type:String,require:true,default:"哈哈哈"}
         3. 可以自定义验证函数  还可以穿具有默认值的函数
         4. 如果是一个对象类型的话，默认类型是一个函数default是因为引用类型的值在一个组件修改其他的地方也会修改
         5. 对大小写不敏感最好是加横线的形式
     
   2. 子组件->父组件  $emit  
      1. 设置自定义事件
      2. 通过$emit传递值  在vue3中可以对自定参数进行验证
      3. vue3在子组件要注册是事件
         1. emits可以是一个数组也可以是一个对象数组 emits:["add",'sub']
         2. emits 也可以是一个对象的写法，对象的写法是对参数的验证
            1. ```
                emits: {
              add: null,
              sub: null,
               addN: (num, name, age) => {
                console.log(num, name, age);
               if (num > 10) {
               return true
              }
              return false;
             }
             },
               ```

3. 非Prop的Attribute的传递
   1. 定义：传递给一个组件某一个属性没有对应的$emit或者props 
   2. 例子：class style id
   3. 继承：
      1. 当有单个根节点：非Prop的Attribute的传递会自动添加到根节点之中
      2. 禁用继承：inheritAttrs: false
      3. 通过$attrs来访问所有的 非props的attribute
      4. 多个根节点的attribute如果没有显示的绑定：那么会报警告，我们必须手动的指定要绑定到哪一个属性

4. 非父子组件之间的通信
   1. Provide/Inject
      1. 满足使用条件：父组件可以作为所有子组件的依赖提供者
      2. 使用：父 provide 子 Inject
      3. 响应式处理  把provide对象变成一个函数就可以改变this指向了使用data里面的数据 
         1. ```js
           provide(){
            return {
               name:"why",
               age:18,
               length:computed(()=>this.name.length)  //变成computed语法成为响应式
            }
           }
            ```
   2. Mitt全局事件总线
      1. vue3在实例中移除了 $on、$off 和 $once
      2. 安装 npm install mitt
      3. 使用：
         1. ```js
            import mitt from 'mitt'
            const emitter =mitt()
            export default emitter
            ```
         2. ```js 
            //监听 home 接收
           import emitter from ./eventBus
           export default{
            created(){
               emitter.on('why',(info)=>{
                  console.log("wgy event" ,info)
               })
            }
           }
            ```
         3. ```js 
            //触发 传值
           import emitter from ./eventBus
           export default{
            components:{
              Home
            },
            methods:{
             triggerEvent(){
               emitter.emit("why",{name:'why',age:18})
             }
            }
           }
            ```
         4. ```js 
            //取消监听
          emitter.all.clear()
           function  onfoo() {}
           emitter.on("foo",onfoo) //监听
           emitter.off('foo',onFoo)  //取消监听
            ```

5. slot插槽
   1. 定义：抽取共性 预留不同  共同元素依然在组件内进行封装，不同元素使用solt作为占用符
   2. 多个插槽
   3. 具名插槽
   4. 动态插槽
   5. 作用域插槽
      1. 渲染作用域
         1. 父模板的作用域都在父作用域编译
         2. 子模板的作用域都在子作用域编译
   6. 独占默认插槽的缩写
   7. 默认插槽和具名插槽混合

6. 切换组件
   1. 使用v-if切换组件
   2. 动态组件是使用 component 组件，通过一个特殊的attribute is 来实现
      1. 可以是通过component函数注册的组件
      2. 在一个组件对象的components对象中注册的组件
   3. 传值：属性和监听事件放到component上来使用

7. 认识keepalive
   1. 属性：
      1. include - string | RegExp | Array。只有名称匹配的组件会被缓存
      2. exclude - string | RegExp | Array。任何名称匹配的组件都不会被缓存
      3. max - number | string。最多可以缓存多少组件实例，一旦达到这个数字，那么缓存组件中最近没有被访问的实例会被销毁
      4. include 和 exclude prop 允许组件有条件地缓存
      5. 生命周期：activated 和 deactivated 这两个生命周期钩子函数来监听

8. Webpack的代码分包
   1. import("./unit/math.js").then(({sum})=>{console.log(sum)})

9.  vue中实现异步组件
   2.  defineAsyncComponent需要返回一个promise
       1.  ```js
          import {defineAsyncComponent} from 'vue'
          const AsyncHome =defineAsyncComponent(()=>import("./AsyncHome.vue"))
          export default{
            components:{
               AsyncHome
            }
          }
           ```
      1.  ```js
          import {defineAsyncComponent} from 'vue'
          const AsyncHome =defineAsyncComponent({
            //工厂函数
            loader:()=>import ('./AsyncHome.vue')
            //加载过程中显示的组件
            loadingComponent:Loading,
            //加载时显示错误组件
            errorComonent:Error,
            delay:2000,
            suspensible:true
          })
           ```
   3. 异步组件和Suspense
      1. 属性：
         1. default：如果default可以显示，那么显示default的内容
         2. fallback：如果default无法显示，那么会显示fallback插槽的内容
      2. 定义：Suspense是一个内置的全局组件

10.   $refs的使用
    1.  有注册过 ref attribute 的所有 DOM 元素和组件实例
11.  生命周期
     1.  是一些钩子函数，在某个时间内会被vue的源码回调
     2.  beforecreated  数据挂载前
     3.  created  数据挂载后
     4.  beforeMounted  dom更新前
     5.  mounted   dom更新后
     6.  befoUnmounted  不会回调  卸载前  例： v-if卸载
     7.  unmounted 不会回调 卸载后   已经注册是的事件取消掉
     8.  beforeUpdate 不会回调 更新 数据更新前的数据
     9.  updated 不会回调  更新后 数据更新后的数据
     10. activated  在缓存的组件想要保留一些状态需要使用
     11. deactivated  在缓存的组件想要保留一些状态需要使用
12.  组件上的v-model
     1.   v-model绑定多个属性
     2.   在组件使用v-model="message"  组件会默认绑定:modelValue="message"  并绑定事件 @update:model-value="message= $event"
          1.   子组件固定事件 emits:["update:modelValue","update:title"]   this.$emit("update:modelValue","232332")   this.$emit("update:title","999")  t同时props：{modelValue:"",title:""}
          2.   父组件定义 v-model="message"   可以不用modelValue   v-model:title="title"
          
13.  computed实现
14.  mixin 组件
     1. 作用： 组件和组件之间有时候会存在相同的代码逻辑，我们希望对相同的代码逻辑进行抽取
     2. 基本使用：
     3. Mixin对象中的选项和组件对象中的选项发生了冲突
        1. 如果是data函数的返回值对象，返回对象会进行合并，如果是对象属性发生冲突，保留组件自身属性
        2. 如果是生命周期钩子会被合并到数组中
        3. 值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象，
           1. methods选项，并且都定义了方法，那么它们都会生效
           2. 果对象的key相同，那么会取组件对象的键值对
     4. 全局混入Mixi应用app的方法 mixin 来完成注册
        1. 那么全局混入的选项将会影响每一个组件
15.  Vue的transition动画
     1.   原理：
          1.   .自动嗅探目标元素是否应用了CSS过渡或者动画，如果有，那么在恰当的时机添加/删除 CSS类名
          2.   如果 transition 组件提供了JavaScript钩子函数，这些钩子函数将在恰当的时机被调用
          3.   如果没有找到JavaScript钩子并且也没有检测到CSS过渡/动画，DOM插入、删除操作将会立即执行
     2. 属性class：
        1. v-enter-from：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除
        2. v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数
        3. v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter-from 被移除过渡/动画完成之后移除
        4. v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除
        5. v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数
        6. v-leave-to：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave-from 被删除过渡/动画完成之后移除。
        7. class命名规则：如果没有定义name那么就以v-来开头。如果定义了那么就以 定义的名字开头
     3. 动画效果话可以通过animation来实现
     4. 可以设置类型 type
     5. 属性：duration
        1. number类型：同时设置进入和离开的过渡时间
        2. pobject类型：分别设置进入和离开的过渡时间  {enter：1000，leave：1000}
     6. 属性过度模式：之前设置的内容消失  
        1. in-out: 新元素先进行过渡，完成之后当前元素过渡离开
        2. out-in: 当前元素先进行过渡，完成之后新元素过渡进入
     7. 首次渲染添加动画 appear
     8. 使用第三方动画 
        1. animate.css 
           1. 作用：准备好的、跨平台的动画库为我们的web项目，对于强调、主页、滑动、注意力引导
           2. 使用：
              1. 安装 npm install animate.css
              2. 在main.js中导入animate.css：
              3. 直接使用animate库中定义的 keyframes 动画  直接使用animate库提供给我们的类
        2.  gsap动画库JavaScript来实现一些动画的效果
            1.  使用：
                1.  安装npm install gsap
                 ```js
                 
                 ```
      9. 对列表的渲染transition-group
      10. 














      







  
   

   







   

   