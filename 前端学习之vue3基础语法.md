---
title: 前端学习之vue3基础语法
date: 2022-07-03 17:04:27
tags: vue3
---

# 认识vue

用于构建用户界面的渐进式框架

可以一点点引入



# vue3源码的哪些变化

####源码通过monorepo的形式来管理源代码：

Mono：单个

Repo：repository仓库 

主要是将许多项目的代码存储在同一个repository中；

这样做的目的是多个包本身相互独立，可以有自己的功能逻辑、单元测试等，同时又在同一个仓库下方便管理； 而且模块划分的更加清晰，可维护性、可扩展性更强；

####源码使用TypeScript来进行重写：

 在Vue2.x的时候，Vue使用Flow来进行类型检测；

 在Vue3.x的时候，Vue的源码全部使用TypeScript来进行重构，并且Vue本身对TypeScript支持也更好了； 

# Vue3带来的变化（性能） 

####使用Proxy进行数据劫持 

在Vue2.x的时候，

Vue2是使用Object.defineProperty来劫持数据的getter和setter方法的；

这种方式一致存在一个缺陷就是**当给对象添加或者删除属性时，是无法劫持和监听的**； 

所以在**Vue2.x的时候，不得不提供一些特殊的API，比如$set或$delete，事实上都是一些hack方法**，也增加了 开发者学习新的API的成本；

 p而在Vue3.x开始，Vue使用Proxy来实现数据的劫持，这个API的用法和相关的原理我也会在后续讲到； 

#### 删除了一些不必要的API：

移除了实例上的**$on, $off 和 $once； **

移除了一些特性：**如filter、内联模板等；**

 #### 包括编译方面的优化：

 生成Block Tree、Slot编译优化、diff算法优化； 

# Vue3带来的变化（新的API） 

由Options API 到CompositionAPI：
在Vue2.x的时候，我们会通过Options API来描述组件对象；
Options API包括data、props、methods、computed、生命周期等等这些选项；
存在比较大的问题是多个逻辑可能是在不同的地方：
 比如created中会使用某一个method来修改data的数据，代码的内聚性非常差；
Composition API可以将 相关联的代码放到同一处 进行处理，而不需要在多个Options之间寻找；

Hooks函数增加代码的复用性：
在Vue2.x的时候，我们通常通过mixins在多个组件之间共享逻辑；
但是有一个很大的缺陷就是mixins也是由一大堆的Options组成的，并且多个mixins会存在命名冲突的问题；
在Vue 3 .x中，我们可以通过Hook函数，来将一部分独立的逻辑抽取出去，并且它们还可以做到是响应式的；

具体的好处，会在后续的课程中演练和讲解（包括原理）；

# 如何使用Vue呢 

#### Vue的本质，就是一个JavaScript的库： 

刚开始我们不需要把它想象的非常复杂；

 我们就把它理解成一个已经帮助我们封装好的库；

 在项目中可以引入并且使用它即可。 

#### 那么安装和使用Vue这个JavaScript库有哪些方式呢 

方式一：在页面中通过CDN的方式来引入； 

方式二：下载Vue的JavaScript文件，并且自己手动引入；

 方式三：通过npm包管理工具安装使用它（webpack再讲）；

 方式四：直接通过Vue CLI创建项目，并且使用它； 

# 方式一：CDN引入 

#### 什么是CDN呢？CDN称之为内容分发网络（Content Delivery Network或Content Distribution Network，缩 写：CDN） 

它是指通过 相互连接的网络系统，利用最靠近每个用户的服务器；

 更快、更可靠地将音乐、图片、视频、应用程序及其他文件发送给用户；

来提供高性能、可扩展性及低成本的网络内容传递给用户； 

#### 常用的CDN服务器可以大致分为两种： 

自己的CDN服务器：需要购买自己的CDN服务器，目前阿里、 腾讯、亚马逊、Google等都可以购买CDN服务器 

开源的CDN服务器：国际上使用比较多的是unpkg、 JSDelivr、cdnjs 

#### Vue的CDN引入 

````js
<script src="https://unpkg.com/vue@next"></script>
````

#### vue CDN的本地引入

复制https://unpkg.com/vue@3.2.36/dist/vue.global.js地址将页面已经打包好的vue文件复制到一个js文件里面。使用通过引入就可以了

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app"></div>

    <script src="../js/vue.js"></script>
    <script>
      Vue.createApp({
        template: `
  <div>
    <H2>{{message}}</H2>
    <button @click="increament">+1</button>
     <h2>{{counter}}</h2>
     <button @click="decreament">-1</button>
    </div>
  `,
        //   datavue这里面必须要接收一个函数，接收对象会导致引用组件的时候发生冲突
        data: function () {
          return {
            message: "你好",
            counter: 1000,
          };
        },
        // 还要设置方法
        methods:{
            // 这里是一个对象这个是vue2中的方法vue3兼容vue2的
            increament(){
                this.counter ++
            },
            decreament(){
                this.counter --
            }
        }
      }).mount("#app");
    </script>
  </body>
</html>

```

# 声明式编程和命令式编程

#### 原生开发和Vue开发的模式和特点，我们会发现是完全不同的，这里其实涉及到两种不同的编程范式 

命令式编程和声明式编程 

命令式编程关注的是 “how to do”，声明式编程关注的是 “what to do”，由框架(机器)完成 “how”的过程 

#### 在原生的实现过程中，我们是如何操作的呢 

 我们每完成一个操作，都需要通过JavaScript编写一条代码，来给浏览器一个指令 

 这样的编写代码的过程，我们称之为命令式编程 

在早期的原生JavaScript和jQuery开发的过程中，我们都是通过这种命令式的方式在编写代码的 

#### 在Vue的实现过程中，我们是如何操作的呢 
我们会在createApp传入的对象中声明需要的内容，模板template、数据data、方法methods 这样的编写代码的过程，我们称之为是声明式编程 

目前Vue、React、Angular的编程模式，我们称之为声明式编程 

# MVVM模型 

#### MVC和MVVM都是一种软件的体系结构 

MVC是Model – View –Controller的简称，是在前期被使用非常框架的架构模式，比如iOS、前端； 

MVVM是Model-View-ViewModel的简称，是目前非常流行的架构模式 

#### 我们也经常称Vue是一个MVVM的框架 

Vue官方其实有说明，Vue虽然并没有完全遵守MVVM的模型，但是整个设计是受到它的启发的。 





# template属性

 在使用createApp的时候，我们传入了一个对象，接下来我们详细解析一下之前传入的属性分别代表什么含义 

 ####template属性：表示的是Vue需要帮助我们渲染的模板信息：

目前我们看到它里面有很多的HTML标签，这些标签会替换掉我们挂载到的元素（比如id为app 的div）的innerHTML
模板中有一些奇怪的语法，比如 双花括号，比如 @click，这些都是模板特有的语法

####使用template属性

方式一：使用script标签，并且标记它的类型为 x-template
方式二：使用任意标签（通常使用template标签，因为不会被浏览器渲染），设置id

template元素是一种用于保存客户端内容的机制，该内容再加载页面时不会被呈现，但随后可以在运行时使
用JavaScript实例化

#data属性

####data属性是传入一个函数，并且该函数需要返回一个对象

在Vue2.x的时候，也可以传入一个对象（虽然官方推荐是一个函数）
在Vue3.x的时候，必须传入一个函数，否则就会直接在浏览器中报错

####data中返回的对象会被Vue的响应式系统劫持，之后对该对象的修改或者访问都会在劫持中被处理：

所以我们在template中通过 双花括号counter 访问counter，可以从对象中获取到数据
所以我们修改counter的值时，template中的 双花括号中counter也会发生改变

#methods属性

####methods属性是一个对象，通常我们会在这个对象中定义很多的方法

这些方法可以被绑定到 template 模板中；
在该方法中，我们可以使用this关键字来直接访问到data中返回的对象的属性

####对于有经验的同学，在这里我提一个问题，官方文档有这么一段描述

为什么不能使用箭头函数（官方文档有给出解释）？
不使用箭头函数的情况下，this到底指向的是什么？ 如果使用箭头函数就会指向window，

# Mustache双大括号语法 

#### “Mustache”语法 (双大括号) 的文本插值 

data返回的对象是有添加到Vue的响应式系统中 

当data中的数据发生改变时，对应的内容也会发生更新。 

Mustache中不仅仅可以是data中的属性，也可以是一个JavaScript的表达式 

# v-once指令 

当数据发生变化时，元素或者组件以及其所有的子元素将视为静态内容并且跳过 

该指令可以用于性能优化 

 如果是子节点，也是只会渲染一次 

# v-text指令 

 用于更新元素的 textContent： 

# v-html 

默认情况下，如果我们展示的内容本身是 html 的，那么vue并不会对其进行特殊的解析 

p如果我们希望这个内容被Vue可以解析出来，那么可以使用 v-html 来展示

~~~html
    <div id="app"></div>

    <template id="my-app">
      <h2>{{message}}</h2>
      <div v-html="info">

      </div>
    </template>
    <script src="../js/vue.js"></script>
    <script>
      const App = {
        template: "#my-app",
        data() {
          return {
            message: "helloWorld",
            info:`<span style="color:red;font-size:20px;">哈哈</span>`
          };
        },
      };

      Vue.createApp(App).mount("#app");
    </script>
~~~

 # v-pre 

v-pre用于跳过元素和它的子元素的编译过程，显示原始的Mustache标签： 

跳过不需要编译的节点，加快编译的速度 展示原始指令

# v-cloak

和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到组件实 例准备完毕。 

# v-bind的绑定属性 

前端讲的一系列指令 主要是将值插入到模板内容中 

除了内容需要动态来决定外，某些属性我们也希望动态来绑定。

 p比如动态绑定a元素的href属性；

 p比如动态绑定img元素的src属性； 

用法 ：动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。 

#### v-bind用于绑定一个或多个属性值，或者向另一个组件传递props值 

比如图片的链接src、网站的链接href、动态绑定一些类 

# 绑定class介绍 

在开发中，有时候我们的元素class也是动态的 ，

当数据为某个状态时，字体显示红色

当数据另一个状态时，字体显示黑色 

绑定class有两种方式： 对象语法 ，数组语法 

# 绑定style介绍 

用v-bind:style来绑定一些CSS内联样式 

这次因为某些样式我们需要根据数据动态来决定 

比如某段文字的颜色，大小等等 

可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名 

绑定class有两种方式 ：对象语法 数组语法 

# 动态绑定属性 

属性名称不是固定的，我们可以使用 :[属性名]=“值” 的格式来定义； 种绑定的方式，我们称之为动态绑定属性 

# v-on绑定事件 

 v-on的使用： 

 缩写：@ p

预期：Function | Inline Statement | Object p 参数：event

  修饰符：

 ü .stop - 调用 event.stopPropagation()。

 ü .prevent - 调用 event.preventDefault()。

 ü .capture - 添加事件侦听器时使用 capture 模式。

 ü .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调

 ü .{keyAlias} - 仅当事件是从特定键触发时才触发回调。

 ü .once - 只触发一次回调。

 ü .left - 只当点击鼠标左键时触发。

 ü .right - 只当点击鼠标右键时触发。

 ü .middle - 只当点击鼠标中键时触发。

 ü .passive - { passive: true } 模式添加侦听器 

用法：绑定事件监听 

# v-if、v-else、v-else-if 

v-if、v-else、v-else-if用于根据条件来渲染某一块的内容 

这些内容只有在条件为true时，才会被渲染出来 

这三个指令与JavaScript的条件语句if、else、else if类似 

 v-if的渲染原理： pv-if是惰性的；

 p当条件为false时，其判断的内容完全不会被渲染或者会被销毁掉； 

p当条件为true时，才会真正渲染条件块中的内容 

# tempalte元素

 因为v-if是一个指令

所以必须将其添加到一个元素上：

 但是如果我们希望切换的是多个元素呢？ 

此时我们渲染div，但是我们并不希望div这种元素被渲染；

 这个时候，我们可以选择使用template；

 template元素可以当做不可见的包裹元素，并且在v-if上使用，但是最终template不会被渲染出来： 

有点类似于小程序中的block template元素 

# v-show 

v-show和v-if的用法看起来是一致的，也是根据一个条件决定是否显示元素或者组件 

在用法上的区别 ：

v-show是不支持template； pv-show不可以和v-else一起使用； 

本质的区别 ：

pv-show元素无论是否需要显示到浏览器上，它的DOM实际都是有渲染的，只是通过CSS的display属性来进行 切换 

pv-if当条件为false时，其对应的原生压根不会被渲染到DOM中 



如果我们的原生需要在显示和隐藏之间频繁的切换，那么使用v-show 

如果不会频繁的发生切换，那么使用v-if 

# v-for

####v-for的基本格式是 "item in 数组" 

of可以代替in

数组通常是来自data或者prop，也可以是其他方式； 

item是我们给每项元素起的一个别名，这个别名可以自定来定义； 

#### 在遍历一个数组的时候会经常需要拿到数组的索引 

如果我们需要索引，可以使用格式： "(item, index) in 数组"；

 注意上面的顺序：数组元素项item是在前面的，索引项index是在后面的 

#### v-for也支持遍历对象，并且支持有一二三个参数 

一个参数： "value in object"; 

二个参数： "(value, key) in object"; 

三个参数： "(value, key, index) in object"; 

#### v-for同时也支持数字的遍历 

每一个item都是一个数字 

# template元素 

你可以使用 template 元素来循环渲染一段包含多个元素的内容 

我们使用template来对多个元素进行包裹，而不是使用div来完成 

# 数组更新检测 

#### Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括： 

push()

 ppop()

 pshift() 

punshift()

 psplice() 

psort() 

reverse() 

#### 替换数组的方法 

上面的方法会直接修改原来的数组，但是某些方法不会替换原来的数组，而是会生成新的数组，比如 filter()、 concat() 和 slice() 

# v-for中的key是什么作用？ 

在使用v-for进行列表渲染时，我们通常会给元素或者组件绑定一个key属性 

#### n 这个key属性有什么作用呢？我们先来看一下官方的解释 

key属性主要用在Vue的虚拟DOM算法，在新旧nodes对比时辨识VNodes 

如果不使用key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法 

而使用key时，它会基于key的变化重新排列元素顺序，并且会移除/销毁key不存在的元素； 

#### 官方的解释对于初学者来说并不好理解，比如下面的问题 

什么是新旧nodes，什么是VNode？ 

没有key的时候，如何尝试修改和复用的？ 

有key的时候，如何基于key重新排列的？ 

#### 因为目前我们还没有比较完整的学习组件的概念，所以目前我们先理解HTML元素创建出来的VNode 

因为目前我们还没有比较完整的学习组件的概念，所以目前我们先理解HTML元素创建出来的VNode 

VNode的全称是Virtual Node，也就是虚拟节点 

事实上，无论是组件还是元素，它们最终在Vue中表示出来的都是一个个VNode 

**VNode的本质是一个JavaScript的对象 V-node是JavaScript**

**vnode的好处就是跨平台渲染**

#### 虚拟DOM 

如果我们不只是一个简单的div，而是有一大堆的元素，那么它们应该会形成一个VNode Tree 

**v-node是一个虚拟节点**

**v-dom是多个虚拟节点形成的一个树结构**



#### 插入F的案例 

**这个案例是当我点击按钮时会在中间插入一个f**；

**我们可以确定的是，这次更新对于ul和button是不需要进行更新，需 要更新的是我们li的列表： **

Ø 在Vue中，对于相同父元素的子元素节点并不会重新渲染整个列 表；

 Ø 因为对于列表中 a、b、c、d它们都是没有变化的；

 Ø 在操作真实DOM的时候，我们只需要在中间插入一个f的li即可；

**那么Vue中对于列表的更新究竟是如何操作的呢？**

 Ø Vue事实上会对于有key和没有key会调用两个不同的方法；

 Ø 有key，那么就使用 patchKeyedChildren方法；

 Ø 没有key，那么久使用 patchUnkeyedChildren方法；  

**新的vnode和旧的vnode做的一个对比就是diff算法**

# 没有key的情况下

#### diff源码解析

获取新的节点和旧的节点

新的节点和旧的节点做出对比

如果旧的节点比新的节点长度短，那么就将旧的新增的插入新的节点并重新更新后面的节点

如果旧的节点长度大于新的节点就创建新的节点

# 有key的情况

**diff源码解析**

从头部开始循环遍历while循环

如果节点类型相同和对应的key相同就会继续遍历  i++

节点不同就跳出循环

从尾部开始遍历循环while循环 

如果节点相同和key相同就继续遍历 e--

如果节点不同就跳出循环  

如果新的节点比较多就设置空将多的节点的null挂载上去

如果是旧的节点比较多就将少的节点卸载

如果是顺序变化了就拿到key保持key的不变将节点移动的变化的key

**算法红黑树删除操作**

mount div渲染到真是dom的过程叫挂载

unmount  卸载 div

更新是两个比较




