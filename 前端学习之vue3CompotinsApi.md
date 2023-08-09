---
title: 前端学习之vue3CompotinsApi
date: 2022-07-19 18:07:10
tags: vue3
---

# Compositions-Api
1.  Options API的弊端
    1.  模块对应 例：data定义数据、methods中定义方法
    2.  不足：
        1.  实现某一个功能逻辑会被拆分到各个属性之中 
        2.  组件变得更大、更复杂时，逻辑关注点的列表就会增长，那么同一个功能的逻辑就会被拆分的很分散
2. setup
   1. 参数 :
      1. props:它其实就是父组件传递过来的属性会被放到props对象中，我们在setup中如果需要使用，那么就可以直接通过props参数获取
         1. 定义props的类型，我们还是和之前的规则是一样的，在props选项中定义
         2. 在template中依然是可以正常去使用props中的属性，比如message
         3. 在setup函数中想要使用props，那么不可以通过 this 去获取
         4. props有直接作为参数传递到setup函数中，所以我们可以直接通过参数
      2. ontext，我们也称之为是一个SetupContext
         1. 参数：attrs：所有的非prop的attribute
         2. slots：父组件传递过来的插槽
         3. emit：当我们组件内部需要发出事件时会用到emit
   2. 返回值：
      1. setup的返回值可以在模板template中被使用
      2. 也就是说我们可以通过setup的返回值来替代data选项 返回一个执行函数来代替在methods中定义的方法
   3. setup不可以使用this  
      1. 组件实例已经创建出来了，
      2. this并没有指向当前组件实例 在setup被调用之前，data、computed、methods等都没有解析，所以在setup中不能获取this
   4. setup中定义的数据提供响应式的特性
      1. ```js
          const  state=reactive({
            name:"an31742",
            counter:1000
          })
         ````
      2. 原理 
         1. 使用reactive函数处理我们的数据之后，数据再次被使用时就会进行依赖收集
         2. 数据发生改变时，所有收集到的依赖都是进行对应的响应式操作（比如更新界面）
         3. data选项，也是在内部交给了reactive函数将其编程响应式对象的
   5. Ref Api
      1. 作用： ref 会返回一个可变的响应式对象，该对象作为一个 响应式的引用 维护着它内部的值，这就是ref名称的来源
         1. 在ref的 value 属性中被维护的  
         2. setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，我们依然需要使用 ref.value的方式
      2. Ref自动解包
         1. 模板中的解包是浅层的解包
         2. 将ref放到一个reactive的属性当中，那么在模板中使用时，它会自动解包
      3. 参数 readonly 
         1. 作用：在Ref或者reactive 传给其他组件在另一个地方可以被使用但是不能被修改
         2. 原理:readonly会返回原生对象的只读代理（也就是它依然是一个Proxy，这是一个proxy的set方法被劫持，并且不能对其进行修改）
         3. 常见的三个类型得参数
            1. 普通对象
            2. reactive返回的对象
            3. ref的对象
         4. 使用：readonly处理的原来的对象是允许被修改的
            1. const info = readonly(obj)，info对象是不允许被修改的
            2. 当obj被修改时，readonly返回的info对象也会被修改
            3.  但是我们不能去修改readonly返回的对象info
         5.  本质：readonly返回的对象的setter方法被劫持了而已
         6.  ```js
              const info={
               name:"why",
               age:18,
              }
              const state1=readonly(info)
              console.log(state1)
              const state=reactive({
               name:"why",
               age:18
              })
             const state2 = readonly(state)

             const nameRef=ref('why')
             const state3=readonly(nameRef)
             ```
      4. Reactive判断的API
         1. isProxy 检查对象是否是由 reactive 或 readonly创建的 proxy
         2. isReactive 检查对象是否是由 reactive创建的响应式代理 如果该代理是 readonly 建的，但包裹了由 reactive 创建的另一个代理，它也会返回 true；
         3. isReadonly  检查对象是否是由 readonly 创建的只读代理
         4. toRaw  返回 reactive 或 readonly 代理的原始对象
         5. shallowReactive 创建一个响应式代理，它跟踪其自身 property 的响应性，但不执行嵌套对象的深层响应式转换 (深层还是原生对象)
         6. shallowReadonly 创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换
      5. toRefs
         1. 目的：当响应式对象做解构的时候使用
         2. S6的解构语法，对reactive返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改reactive返回的state对象，数据都不再是响应式
         3. toRefs的函数，可以将reactive返回的对象中的属性都转成ref   name 和 age 本身都是 ref的 state.name和ref.value之间建立了 链接，任何一个修改都会引起另外一个变化
         4. 作用：一个reactive对象中的属性为ref          
         5. ```js
            const state =reactive({
               name:'why',
               age:18
            })
            const {name,age}=toRefs(state) 
            age.value
             const changeAge = () => {
                age.value++;
              }
            //将解构数值变成响应式
             ```
      6. ref其他的API
         1. unref 获取一个ref引用中的value，那么也可以通过unref
         2. isRef 判断值是否是一个ref对象
         3. shallowRef 创建一个浅层的ref对象
         4. triggerRef 手动触发和 shallowRef 相关联的副作用
         5. ```js
            const info =shallowRef({name:'why',})
            const changeInfo =()=>{
               info.value.name="coderWhy"
               triggerRef(info)
            }
            ``` 
      7. customRef
         1. 作用：创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制
         2. 使用：一个工厂函数，该函数接受 track 和 trigger 函数作为参数 该返回一个带有 get 和 set 的对象
   6. computed
      1. setup 函数中使用 computed 方法来编写一个计算属性
      2. ```js
           const fullName=computed({
            get:()=>{
               return firstName.value + '' +lastName.value
            },
            set:newValue=>{
               const names =newValue.split(" ")
               fileName.value=names[0]
               lastName.value=names[1]
            }
           })
            ``` 
   7. 侦听器
      1. watchEffect用于自动收集响应式数据的依赖
         1. 条件：
            1. watchEffect传入的函数会被立即执行一次，并且在执行的过程中会收集依赖
            2. 只有收集的依赖发生变化时，watchEffect传入的函数才会再次执行
         2. watchEffect的停止侦听：watchEffect的返回值函数，调用该函数
         3. watchEffect清除副作用
            1. 在网络请求还没到达的时候再次停止侦听器，侦听器侦听函数被再次执行了，上一次网络请求应该取消掉
            2. watchEffect传入的函数被回调时可以获取到一个参数：onInvalidate 副作用即将重新执行 或者 侦听器被停止 时会执行该函数传入的回调函数
            3. **在setup中使用ref**
            4. ```js
              const stopWatch=watchEffect(()=>{
               console.log("watchEffect执行",name.value,age.value)
              })
              const changeAge=()=>{
               age.value++,
               if(age.value>20){
                  stopWatch()
               }
              }
               ``` 
      2. watch需要手动指定侦听的数据源
         1. watch和watchEffect相比的优点
            1. 懒执行副作用（第一次不会直接执行）
            2. 更具体的说明当哪些状态发生变化时，触发侦听器的执行
            3. 访问侦听状态变化前后的值
         2. watch侦听函数的数据源有两种类型：
            1. getter：该getter必须引用可响应式对象(reactive,ref)
             1. ```js
                const state =reactive({
                  name:"why",
                  age:18
                })

                watch(()=>state.name,(newValue,oldValue)=>{
                  console.log(newValue,oldValue)
                })

                const changeName=()=>{
                  state.name="coderWhy"
                }
               ```
            2.  ```js
                1.  //reactive或者ref（比较常用的是ref）
                const name =ref("kobe")

                watch(name,(newValue,oldValue)=>{
                  console.log(newValue,oldValue)
                })

                const changeName=()=>{
                  state.name="jams"
                }
               ```
            3.  ```js
                1.  //可以监听多个数据源
                const name =ref("kobe")
                const age =ref(18)

                 const changeName=()=>{
                  state.value="jams"
                }
                watch([name,age],(newValue,oldValue)=>{
                  console.log(newValue,oldValue)
                })

               
               ```
            4.  ```js
                1.  //可以监听一个数组或者一个对象那么可以使用getter函数
                const names =reactive(["ABD",'CNCS','JHNK'])

              
                watch(()=>[...name],(newValue,oldValue)=>{
                  console.log(newValue,oldValue)
                })

                const changeName=()=>{
                  names.push="jams"
                }
               ```
            5.  ```js
                1.  //可以设置深度监听
                const state =reactive({
                  name:"why",
                  age:18,
                  friend:{
                     name:'kobe'
                  }
                })

              
                watch(()=>state,(newValue,oldValue)=>{
                  console.log(newValue,oldValue)
                },{deep:true,immediate:true})

                const changeName=()=>{
                  state.friend.name='aaa'
                }
               ```
   8. 生命周期钩子
      1. 可以直接导入onMount来注册  onCreated来注册  生命周期可以注册多
      2. beforecreated 和created没有生命周期，在hook setUP可以直接石述思用
   9.  provide函数
      3.  参数：
          1.  name：提供的属性名称
          2. value：提供的属性值
      4.  ```js
               
               let counter=100
               let info={
                  name:"why",
                  age:100
               }

              
               provide("counter",counter)
               provide("info",info)
               ```
   10. Inject函数
       1.  参数：
           1.  要inject的property的name
           2.  默认值
           3.   ```js
               const counter=inject("counter")
               const info=inject("info")
             ```
   11. 数据响应式
       1.   ```js
            1.   //provide 值和 inject 值之间的响应性，我们可以在 provide 值时使用 ref 和 reactive
          let counter =ref(100)
          let info=reactive({
            name:'why',
            age:18
          })
          provide("counter",counter)
          provide("info",info)
             ```
   12. 修改响应式property
       1.   ```js
            1.   //修改可响应的数据，那么最好是在数据提供的位置来修改，修改方法进行共享，在后代组件中进行调用
          
          const changeInfo=()=>{
            info.name="codewhy"

          }
          provide("changeInfo",changeInfo)
           ```
   13. useCounter
       1.    ```js
            1.  //对之前的counter逻辑进行抽取
          
          import {ref} from 'vue'
          export function useCounter(){
            const counter =ref(0)

            const increment =()=>counter.value++
            const decrement =()=>counter.value--
            return{
               cpunter,
               increment,
               decrement
            }
          }
          ```
   14. useTitle
       1.   ```js
            1.  //对之前的counter逻辑进行抽取
          
          import {ref,watch} from 'vue'
          export function useTitle(){
            const titelRef =ref(title)

           watch(titelRef,(newValue)=>{
            document.title=newValue
           },{
            immediate:true
           })

           return titelRef
          }
          ```
   15. useScrollPostion
       1.    ```js
            1.  //对之前的counter逻辑进行抽取
          
          import {ref} from 'vue'
          export function useScrollPostion(){
            const scrollX =ref(0)
            const scrollY =ref(0)

           document.addEventListener("scroll",()=>{
            scrollX.value=window.scrollX
            scrollY.value=window.scrollY
           })

           return {scrollX,scrollY}
          }
          ```
   16. useMousePostion
       1.    ```js
            1.  //对之前的counter逻辑进行抽取
          
          import {ref} from 'vue'
          export function useScrollPostion(){
            const mouseX =ref(0)
            const mouseY =ref(0)

           document.addEventListener("scroll",()=>{
            mouseX.value=window.mouseX
            mouseY.value=window.mouseY
           })

           return {mouseX,mouseY}
          }
          ```
   17. useLocalStorage
       1.     ```js
            1.  //对之前的counter逻辑进行抽取
          
          import {ref,watch} from 'vue'
          export function useLocalStorage(key,defaultValue){
           const data=ref(defaultValue)

          if (defaultValue) {
            window.localStorage.setItem(key,JSON.stringify(defaultValue))
          }else{
            data.value=JSON.stringify(window.localStorage.getItem(key)
          }
          watch(data,()=>{
            window.localStorage.setItem(key,JSON.stringify(data.value))
          })
          
           return  data
          }
          ```


         


 
 






   
