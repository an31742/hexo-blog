---
title: 前端学习之vue3高级语法
date: 2022-07-19 22:02:22
tags: vue3
---

# vue3高级语法
1.  h函数
    1.  作用：创建 vnode 的一个函数 完整语法 createVNode() 函数
    2.  使用地方：render函数  setup函数
        1.   ```js
             import {h} from 'vue'
             
             export default{
                render() {
                    return h('div',{class:"app"},"hello App")
                },
             }
            ```
        2.  ```js
             import {h} from 'vue'
             
             export default{
                setup() {
                    return h('div',{class:"app"},"hello App")
                },
             }
            ```
2. jsx在babel中的配置
   1. 安装：npm install @vue/babel-plugin-jsx -D
   2. 在babel.config.js配置文件
      1. ```js
             module.exports={
              presest:[
                '@vue/cli-plgugin-babel/presest'
              ],
              plugin:[
                "@vue/babel-plgugin-jsx"
              ]
             }

              export.default{
               render(){
                return (<div>hello jsx</div>)
               }

              }
            
            ```
3. 自定义指令
   1. 使用条件：对DOM元素进行底层操作，这个时候就会用到自定义指令
   2. 使用方法：
      1. 组件中通过 directives 选项，只能在当前组件中使用
      2. app的 directive 方法，可以在任意组件中被使用
      3.   ```js
            //当某个元素挂载完成后可以自定获取焦点
           //实现方式1  默认的实现方式
          <template>
             <div>
               <input  type="text" ref="inputRef">
             </div>
          </template>

           <script>
             import {setup,onMounted} from 'vue'
              export.default={
                setup(){
                    const inputRef=ref(null)

                    onMounted(()=>{
                       inputRef.value.focus() 
                    })
                    return {inputRef}
                }
              }
           </script>
           //实现方式2 v-focus 的局部指令,在组件选项中使用 directives 在对象中编写我们自定义指令的名称,自定义指令有一个生命周期，是在组件挂载后调用的 mounted
           <script>
           
              export.default={
               directives:{
                 focus:{
                    mounted(el){
                        el.focus()
                    }
                 }
               }
              }
           </script>

           //实现方式3  v-focus 的全局指令

        
            app.directive("focus",{
                mounted(el){
                    el.focus()
                }
            })
           ```
    3. 指令的生命周期
       1. created：在绑定元素的 attribute 或事件监听器被应用之前调用
       2. beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用
       3. mounted：在绑定元素的父组件被挂载后调用
       4. beforeUpdate：在更新包含组件的 VNode 之前调用
       5. updated：在包含组件的 VNode 及其子组件的 VNode 更新后调用
       6. beforeUnmount：在卸载绑定元素的父组件之前调用
       7. unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次
    4. 指令的参数和修饰符
       1.  ```js
             <button v-why:info.aaa.bbb="{name:"coderWhy",age:18}">{{counter}}</button>
          //  info是参数的名称  aaa-bbb是修饰符的名称  后面是传入的具体的值
            
            ```
    5. 时间格式化自定义指令
       1. ```js
           import dayjs from 'dayjs'

           export default function(app){
              let formate="YYYY-MM-DD HH:MM:ss"
              app.directive('formate.time',{
                created(el,bingings){
                    if (bingings.value) {
                     formate= bingings.value
                    }
                }
                mounted(el){
                    const textContent =el.textContent
                    let timesTamp=parseInt(el.textContent)
                    if (textContent.length===10) {
                        timesTamp=timesTamp*10000
                    }
                    console.log("timesTamp",timesTamp)
                    el.textContent=dayjs(timesTamp).format(format)
                }
              })
          
           }
            
            ```
# telePort
   1.  使用条件：组件a在组价b中使用组件a中的template元素会被挂载到组件b中。不希望被挂载到组件树上就可以通过teleport来完成
   2.  属性：
       1.  to：指定将其中的内容移动到的目标元素，可以使用选择器
       2.  disabled：是否禁用 teleport 的功能
   3. 在 teleport 中使用组件，并且也可以给他传入一些数据
   4. 多个teleport
      1. 多个teleport应用到同一个目标上（to的值相同），那么这些目标会进行合并
# vue插件
   1.  作用：像vue全局添加一些功能时就会使用插件
   2.  编写方式：
       1.  ```js
            // p对象类型：一个对象，但是必须包含一个 install 的函数，该函数会在安装插件时执行
            export default{
                name:why,
                install(app,options){
                    console.log("插件安装",app,options)
                    console.log(this.name)
                }
            }
            
            ```
       2.  ```js
            // 函数类型：一个function，这个函数会在安装插件时自动执行
            export default function(app,options){
                    console.log("插件安装",app,options)    
                
            }
           ```  

   3. 功能：
      1. 添加全局方法或者 property，通过把它们添加到 config.globalProperties 上实现
      2. p添加全局资源：指令/过滤器/过渡等
      3. p添加全局资源：指令/过滤器/过渡等
   

   

