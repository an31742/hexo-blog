---
title: 前端学习之vue3源码学习
date: 2022-07-19 22:03:16
tags: vue3
---

# vue3源码学习
1.  虚拟Dom
    1.  虚拟dom对真实dom的抽象的好处： 
        1.  对真实的元素节点进行抽象抽象程虚拟节点：
            1.  直接操作dom有很多限制 diff 和 clone
            2.  可以使JavaScript表达很多逻辑
        2. 可以方便实现跨平台
           1. 如渲染在canvas、WebGL、SSR、Native（iOS、Android）上
           2. 并且Vue允许你开发属于自己的渲染器（renderer），在其他的平台上渲染
    2. 虚拟dom渲染的过程
       1. template节点
       2. render函数 
       3. Vnode虚拟节点
       4. 真实元素
       5. 最终浏览器展示
2. Vue源码三大核心模块
   1. Compiler模块：编译模板系统
   2. Runtime模块：也可以称之为Renderer模块，真正渲染的模块
   3. Reactivity模块：响应式系统
   4. 三个模块是如何协同工作的
3. 实现一个mini版的vue
   1. 渲染系统模块
      1.  h函数，用于返回一个VNode对象
          1. ```js
               const h=(tag,props,children)=>{
               return{
                tag,
                props,
                children
              }
            }

           ```
      2. mount函数的实现
         1.  ```js
           
             1.根据tag，创建HTML元素，并且存储到vnode的el中
            2.处理props属性，如果以on开头，那么监听事件，普通属性直接通过 setAttribute 添加即可
             3.处理子节点，如果是字符串节点，那么直接设置textContent，如果是数组节点，那么遍历调用 mount 函数
             const mount=(vnode,container)=>{

            const el =vnode.el=document.createElemnt(vnode.tag)
            if(vode.props){
                for(const key in vnode.props){
                    const value=vnode.props[key]
                    if(key.startsWith('on')){
                        el.addEventListener(key.slice(2).toLowerCase(),value)
                    }else{
                        el.setAttribute(key,value)
                    }
                }
            }

            if(vnode.children){
             if(typeof vnode.children==="string"){
                el.textContent=vnode.children
             }else{
                vnode.children.forEach(child=>{
                    mount(child,el)
                })
             }
            }
            container.appendChild(el)
          }
          ```
      3. patch函数，用于对两个VNode进行对比，决定如何处理新的VNode
         1. n1和n2是不同类型的节点,找到n1的el父节点，删除原来的n1节点的e,挂载n2节点到n1的el父节点上
         2. n1和n2节点是相同的节点
            1. 处理props的情况,先将新节点的props全部挂载到el上,判断旧节点的props是否不需要在新节点上，如果不需要，那么删除对应的属性
            2. 处理children的情况,
               1. 如果新节点是一个字符串类型，那么直接调用 el.textContent = newChildren
               2. 如果新节点不同一个字符串类型
                  1. 旧节点是一个字符串类型,将el的textContent设置为空字符串,就节点是一个字符串类型，那么直接遍历新节点，挂载到el上
                  2. 旧节点也是一个数组类型,1.取出数组的最小长度,2.遍历所有的节点，新节点和旧节点进行path操作,3如果新节点的length更长，那么剩余的新节点进行挂载操作,4如果旧节点的length更长，那么剩余的旧节点进行卸载操作
         3.  ```js
              const patch = (n1, n2) => {
                 if (n1.tag !== n2.tag) {
                     const n1ElParent = n1.el.parentElement;
                     n1ElParent.removeChild(n1.el);
                    mount(n2, n1ElParent);
                  } else {
               // 1.取出element对象, 并且在n2中进行保存
               const el = n2.el = n1.el;

               // 2.处理props
               const oldProps = n1.props || {};
               const newProps = n2.props || {};
               // 2.1.获取所有的newProps添加到el
               for (const key in newProps) {
                 const oldValue = oldProps[key];
                 const newValue = newProps[key];
                 if (newValue !== oldValue) {
                   if (key.startsWith("on")) { // 对事件监听的判断
                     el.addEventListener(key.slice(2).toLowerCase(), newValue)
                   } else {
                     el.setAttribute(key, newValue);
                   }
                 }
               }

               // 2.2.删除旧的props
               for (const key in oldProps) {
                 if (key.startsWith("on")) { // 对事件监听的判断
                   const value = oldProps[key];
                   el.removeEventListener(key.slice(2).toLowerCase(), value)
                 } 
                 if (!(key in newProps)) {
                   el.removeAttribute(key);
                 }
               }

               // 3.处理children
               const oldChildren = n1.children || [];
               const newChidlren = n2.children || [];

               if (typeof newChidlren === "string") { // 情况一: newChildren本身是一个string
                 // 边界情况 (edge case)
                 if (typeof oldChildren === "string") {
                   if (newChidlren !== oldChildren) {
                     el.textContent = newChidlren
                   }
                 } else {
                   el.innerHTML = newChidlren;
                 }
               } else { // 情况二: newChildren本身是一个数组
                 if (typeof oldChildren === "string") {
                   el.innerHTML = "";
                   newChidlren.forEach(item => {
                     mount(item, el);
                   })
                 } else {
                   // oldChildren: [v1, v2, v3, v8, v9]
                   // newChildren: [v1, v5, v6]
                   // 1.前面有相同节点的原生进行patch操作
                   const commonLength = Math.min(oldChildren.length, newChidlren.length);
                   for (let i = 0; i < commonLength; i++) {
                     patch(oldChildren[i], newChidlren[i]);
                   }

                   // 2.newChildren.length > oldChildren.length
                   if (newChidlren.length > oldChildren.length) {
                     newChidlren.slice(oldChildren.length).forEach(item => {
                       mount(item, el);
                   })
                   }

                   // 3.newChildren.length < oldChildren.length
                   if (newChidlren.length < oldChildren.length) {
                     oldChildren.slice(newChidlren.length).forEach(item => {
                       el.removeChild(item.el);
                     })
                   }
                 }
               }
             }
           }

           ```

   
   2. 可响应式系统模块
      1. ```js
          //vue2实现响应式
          class Dep {
               constructor() {
                 this.subscribers = new Set();
               }

               depend() {
                 if (activeEffect) {
                   this.subscribers.add(activeEffect);
                 }
               }

               notify() {
                 this.subscribers.forEach(effect => {
                effect();
                 })             
               }
             }

             let activeEffect = null;
             function watchEffect(effect) {
               activeEffect = effect;
               effect();
               activeEffect = null;
             }


             // Map({key: value}): key是一个字符串
             // WeakMap({key(对象): value}): key是一个对象, 弱引用
             const targetMap = new WeakMap();
             function getDep(target, key) {
               // 1.根据对象(target)取出对应的Map对象
               let depsMap = targetMap.get(target);
               if (!depsMap) {
                 depsMap = new Map();
                 targetMap.set(target, depsMap);
               }

               // 2.取出具体的dep对象
               let dep = depsMap.get(key);
               if (!dep) {
                 dep = new Dep();
                 depsMap.set(key, dep);
               }
               return dep;
             }


             /             / vue2对raw进行数据劫持
             function reactive(raw) {
               Object.keys(raw).forEach(key => {
                 const dep = getDep(raw, key);
                 let value = raw[key];
             
                 Object.defineProperty(raw, key, {
                   get() {
                     dep.depend();
                     return value;
                   },
                   set(newValue) {
                     if (value !== newValue) {
                       value = newValue;
                       dep.notify();
                     }
                   }
                 })
               })

               return raw;
             }
        ```
     2. ```js
              //vue3响应式
                class Dep {
                constructor() {        
                  this.subscribers = new Set();
                }

                addEffect(effect) {
                  this.subscribers.add(effect);
                }
        
                notify() {
                  this.subscribers.forEach(effect => {
                    effect();
                  })
                 }
               }


              const info = {counter: 100};

              const dep = new Dep();
        
              function doubleCounter() {
                console.log(info.counter * 2);
              }
              
              function powerCounter() {
                console.log(info.counter * info.counter);
              }

              dep.addEffect(doubleCounter);
              dep.addEffect(powerCounter);

              info.counter++;
              dep.notify();
         ```   
   3. 应用程序入口模块
      1. 框架外层api的设计
         1. createApp用于创建一个app对象
         2. 该app对象有一个mount方法，可以将根组件挂载到某一个dom元素上
   4. 阅读源码之挂载根组件
      1.  {% asset_img vueSour.jpg 图片引用方法一 %} 
   5. proxy的优点
      1. Object.definedProperty 是劫持对象的属性时，如果新增元素需要再次调用definedProperty，而 Proxy 劫持的是整个对象，不需要做特殊处理
      2. 使用 defineProperty 时，我们修改原来的 obj 对象就可以触发拦，而使用 proxy，就必须修改代理对象，即 Proxy 的实例才可以触发拦截
      3. Proxy 能观察的类型比 defineProperty 更丰富，has：in操作符的捕获器，deleteProperty：delete 操作符的捕捉器
      4. Proxy 作为新标准将受到浏览器厂商重点持续的性能优化
      5. Proxy 不兼容IE，