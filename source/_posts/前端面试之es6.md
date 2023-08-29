---
title: 前端面试之ES6
date: 2022-07-28 01:26:09
tags: 前端面试
---





### 3，es6有哪些新特性

1. let 和 const
2. 解构赋值
3. 箭头函数
4. 模板字符串
5. 数组扩展
6. promise
7. set和map
8. class类
9. 装饰器

#### 3.1什么是let

- let 定义变量，变量不可以再次定义，但可以改变其值
- 具有块级作用域
- 没有变量提升，必须先定义再使用
- let声明的变量不会压到window对象中，是独立的

#### 3.2什么是const

1. 一旦定义const 常量 必须初始化赋值    
2. const的作用域与let命令相同：只在声明所在的块级作用域内有效。    
3.  const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。 
4. const 变量为 复合类型时，变量指向的内存地址，保存的只是一个指向实际数据的指针，可以为符合类型添加属性或者方法，但是不可以改变指针地址引用  

#### 3.3let和var有什么区别

var : 变量在没有var之前声明之前就可以使用，不过值是 undefind      

let : 变量必须声明之后才可以使用，并且作用域范围在 代码块范围内有效 

var：定义的变量，作用域为包括它的函数作用域或者全局作用域。

let：作用域是在定义它的块级代码以及其中包括的子块中，并且无法在全局作用域添加变量。

```js
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
// let 无法在全局作用域中定义变量
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```



var ：在同一个作用域内，重复声明，在生成执行上下文的时候，会无视后面的声明

let：在同一个作用域内,不可以重复声明 

```js
// 报错
(function () {
  let a = 10;
  var a = 1;
  console.log(a)
})()
// 报错
(function () {
  let a = 10;
  let a = 1;
  console.log(a)
})()
// 报错
(function () {
  var a = 10;
  var a = 1;
  console.log(a) // 1
})()
```

var:将其先进行初始化为undefined，然后再执行代码，也就是所谓的“提升”现象。

let:在代码执行之前的扫描，同样也会对let变量进行“提升”，但并没有将其置为undefined。let定义的变量虽然经历了提升，但在没有执行到初始化它的代码前，该变量并没有被初始化，如果此时访问的话，会被置为[ReferenceError](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/ReferenceError)错误。从代码块开始到执行到let变量初始化完毕这段时间，let变量已经被声明，但不可访问。这段时间被成为临时死区。

#### 3.4什么是promise

1，promise有三种状态

pending（执行中）、success（成功）、rejected（失败 、

* pending 不会触发任何 then catch 回调
* 状态变为 resolved 会触发后续的 then 回调
* 状态变为 rejected 会触发后续的 catch 回调


  

用于解决地狱回调，将异步转化为同步

```js
`let p = new Promise((resolve,reject) => {
    reject('error');
});

p.catch(result => {
    console.log(result);
})`

```

2 Promise有五个常用的方法：then()、catch()、all()、race()、finally。

 then和catch
- then正常返回resloved，里面报错返回rejected
- catch正常返回resloved，里面报错返回rejected
- ```js
      
       // then正常返回resloved 状态的promise
      Promise.resolve().then(()=>{
        return 100
      })
      //then里面抛出错误，会返回rejected
      Promise.resolve().then(()=>{
        throw new Error('err')
      })
      //catch 不抛出错误会返回resolved的promise
        Promise.reject().catch(()=>{
       console.error("catch some error")
      })
   
     //catch 抛出错误会返回rejected的promise
        Promise.reject().catch(()=>{
       console.error("catch some error")
       throw new Error('eerr')
      })

  ```
```js
  // 第一题
Promise.resolve().then(() => {
    console.log(1)
}).catch(() => {
    console.log(2)
}).then(() => {
    console.log(3)
})
//1 3  resolve会触发then  
// 第二题
Promise.resolve().then(() => { // 返回 rejected 状态的 promise
    console.log(1)     //这里抛出一个错误会触发rejected 所以后面接的catch
    throw new Error('erro1')
}).catch(() => {   // 返回 resolved 状态的 promise  
    console.log(2)
}).then(() => {   //这里面接的catch所以会返回reselved 状态所以这里面接then
    console.log(3)
})

//1 2 3
// 第三题
Promise.resolve().then(() => { // 返回 rejected 状态的 promise
    console.log(1)
    throw new Error('erro1')
}).catch(() => { // 抛出一个错误这个后面接是rejected 所以后面接的catch  返回 resolved 状态的 promise
    console.log(2)
}).catch(() => {
    console.log(3)  // 因为上面是catch正常resloved  所以后面应该接then
})

//1 2
```

all() 方法可以完成并行任务， 它接收一个数组，数组的每一项都是一个promise对象。当数组中所有的promise的状态都达到resolved的时候，all方法的状态就会变成resolved，如果有一个状态变成了rejected，那么all方法的状态就会变成rejected

```
 let promise1 = new Promise((resolve,reject)=>{ setTimeout(()=>{ resolve(1); },2000) }); 
 let promise2 = new Promise((resolve,reject)=>{ setTimeout(()=>{ resolve(2); },1000) }); 
 let promise3 = new Promise((resolve,reject)=>{ setTimeout(()=>{ resolve(3); },3000) });
  Promise.all([promise1,promise2,promise3]).then(res=>{ console.log(res); //结果为：[1,2,3] })
```

调用all方法时的结果成功的时候是回调函数的参数也是一个数组，这个数组按顺序保存着每一个promise对象resolve执行时的值

race()
race方法和all一样，接受的参数是一个每项都是promise的数组，但是与all不同的是，当最先执行完的事件执行完之后，就直接返回该promise对象的值。如果第一个promise对象状态变成resolved，那自身的状态变成了resolved；反之第一个promise变成rejected，那自身状态就会变成rejected。

```js
let promise1 = new Promise((resolve,reject)=>{ setTimeout(()=>{ reject(1); },2000) }); 
let promise2 = new Promise((resolve,reject)=>{ setTimeout(()=>{ resolve(2); },1000) }); 
let promise3 = new Promise((resolve,reject)=>{ setTimeout(()=>{ resolve(3); },3000) }); 
Promise.race([promise1,promise2,promise3]).then(res=>{ console.log(res); 
//结果：2 },rej=>{ console.log(rej)}; )
```
那么race方法有什么实际作用呢？当要做一件事，超过多长时间就不做了，可以用这个方法来解决

```
Promise.race([promise1,timeOutPromise(5000)]).then(res=>{})
```
#### 什么是async和await

async和await是将异步强行转换为同步处理。 

async/await与promise不存在谁代替谁的说法，因为async/await是寄生于Promise，Generater的语法糖。 

async和await 用同步的语法编写异步的代码

try catch 相当于 promise catch

**async封装prmise，返回promise对象**
**await是处理promise成功的一种情况。相当于then，try catch是处理失败的一种情况**




```js
   async function fn1(){
      return 100
   }
   const rest1=fn1()  //执行async函数返回的是一个promise对象
   console.log(rest1)  //返回的是一个promise对象
   rest1.then(data=>{
    console.log(data)   //100
   })

  !(async function(){
     const p1=Promise.resolve(100)
    const data=await p1  //await 相当于promisethen
    console.log(data)
  })()

  
  
  !(async function(){
    const data=await 200  //await 相当于promise.resolve(200)
    console.log(data)

  })()

  !(async function(){
    const data=await fn1() //await 后面可以使用函数
    console.log(data)
  })()

  //可以通过try catch来捕获一个错误
   !(async function(){
   const p4 =Promise.reject("err")
   try {
     const res= await p4
     console.log(res)
   } catch (error) {
      console.error(error)
   }
  })()


```

#### async和promise关系吗

* 执行async，返回的是一个promise对象
* await相当于promise的then
* try catch可以捕获异常，代替了promise的catch

async用同步的代码写异步但是改变不了异步的本质

```js
  async function async1 () {
  console.log('async1 start')//2
  await async2()
  console.log('async1 end') //5
   // 关键在这一步，它相当于放在 callback 中，最后执行    await 之后相当于异步
}

async function async2 () {
  console.log('async2') //3
}

console.log('script start') //1
async1()
console.log('script end') //4


//script start   async1 start   async2   script end    async1 end
```
#### for in和for of的区别

* for in常用于同步的遍历
* for of常用于异步的遍历
#### 箭头函数和普通函数有什么区别

箭头函数的特性：

1，箭头函数是匿名函数，自身没有this和arguments，它的this从上下文捕捉而来； 

2，箭头函数不能作为构造函数，和 new 一起用就会抛出错误； 

3，箭头函数没有原型属性（prototype）； 

4，箭头函数不能当做Generator函数,不能使用yield关键字 

箭头函数和普通函数的区别：

1箭头函数的arguments指向其父级函数作用域的arguments

```js
function fun(params) {
  console.log(arguments);
  let a=()=>{
    console.log(111,arguments);
  }
  a(33)
}
fun(222)
//Arguments [222, callee: ƒ, Symbol(Symbol.iterator): ƒ]0: 222callee: ƒ fun(params)length: 1Symbol(Symbol.iterator): ƒ values()__proto__: Object
// 111 Arguments [222, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

2this的指向不同

**普通函数的this指向的是：谁调用该函数就指向谁**

**箭头函数的this指向的是：在你书写代码时候的上下文环境对象的this，如果没有上下文环境对象，那么就指向最外层对象window。**

3箭头函数不能作为构造函数使用   不能new一个新的函数

```js
let Person = () => {
	
};
let obj = new Person(); // 报错，Person is not a constructor
// 换个角度理解，箭头函数中都没有自己的this，怎么处理成员，所以不能当构造函数
```

4，普通函数存在变量提升的现象

```js
faceTo(1)
function faceTo() {
  console.log(111,this);
}

faceTol(2)
var faceTol=()=> {
  console.log(222,this);
}
//报错不是一个函数
```

#### 什么是rest参数

**rest 参数：剩余参数，以 … 修饰最后一个参数，把多余的参数都放到一个数组中。可以替代 arguments 的使用**

```js
function fn(a, b, ...values) {
    console.log(a); // 6
    console.log(b); // 1
    console.log(values); // [100, 9, 10]
}
// 调用
console.log(fn(6, 1, 100, 9, 10));
```

#### 字符串的扩展

1，模板字符串

- 模板字符串解决了字符串拼接不便的问题
- 模板字符串使用反引号 **`** 括起来内容
- 模板字符串中的内容可以换行
- 变量在模板字符串中使用 `${name}` 来表示，不用加 + 符号

```js
let name = 'zs';
let age = 18;
// 拼接多个变量，在模板字符串中使用占位的方式，更易懂
let str = `我是${name}，今年${age}`;

// 内容过多可以直接换行
let obj = {name: 'zhangsan', age: 20};
let arr = ['175cm', '60kg'];
let html = `
	<div>
		<ul>
			<li>${obj.name}</li>
			<li>${obj.age}</li>
			<li>${arr[0]}</li>
			<li>${arr[1]}</li>
		</ul>
	</div>
`;
```

2，trim()

`trim()` 方法可以去掉字符串两边的空白

3，repeat()

`repeat`方法返回一个新字符串，表示将原字符串重复`n`次。

```js
let html = '<li>itheima</li>';
html = html.repeat(10);
```

### 3.10数组的扩展

1，扩展运算符：...也可以看做是 **...** 可以把数组中的每一项展开

```js
// 合并两个数组
let arr1 = [1, 2];
let arr2 = [3, 4];
let arr3 = [...arr1, ...arr2];
console.log(arr3); // [1, 2, 3, 4]

// 把数组展开作为参数，可以替代 apply
// 求数组的最大值
let arr = [6, 99, 10, 1];
let max = Math.max(...arr); // 等同于 Math.max(6, 99, 10, 1);
```

2,Array.from()

- 把伪数组转换成数组
- 伪数组必须有length属性，没有length将得到一个空数组
- 转换后的数组长度，是根据伪数组的length决定的

```js
let fakeArr = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
};

let arr = Array.from(fakeArr);
console.log(arr); // ['a', 'b', 'c']

// 转数组的对象必须有length值，因为得到的数组的成员个数是length指定的个数
// 上例中，如果length为2，则得到的数组为 ['a', 'b']
```

### Set 的成员

- `size`：属性，获取 `set` 中成员的个数，相当于数组中的 `length`
- `add(value)`：添加某个值，返回 Set 结构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `clear()`：清除所有成员，没有返回值

Set的特点就是该对象里面的成员不会有重复。

```js
// 将一些重复的值加入到Set对象中，看看效果
const s = new Set();
// 使用forEach遍历前面的数组，然后将数组中的每个值都通过Set对象的add方法添加到Set对象中
[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
// s = {2, 3, 5, 4}

let arr = [1,1,2,2,3,3]
let set = [...new Set(arr)]


// 遍历Set对象，发现重复的值只有一份
// for...in  循环中的 i 表示数组的下标，或对象的属性名
// for...of  循环中的 i 表示数组的值，或对象的值
for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

###  类的声明
```
function FN(argument) {
}
FN.prototype.hi = function(argument) {
  console.log(hi);
};

【es6】
class FN_1{
  constructor(){
    this.name = "name";
  }
  hi(){  //函数之间没有逗号
    console.log(1);
  }
}
```

#### 类的实例
```
new FN_1();
```

### 类的继承

```
【1.call继承】
缺点：构造函数上的属性就挂载过来了。但是原型上的函数是没有挂载过来。
function P(argument) {
  this.name = "p";
}
P.prototype.p_fn = function() {
  console.log("p_fn");
};

function s_1 () {
  // 构造函数上的属性就挂载过来了。但是原型上的函数是没有挂载过来。
  P.call(this);
}
s_1.prototype.s_fn = function(argument){
  console.log("s_fn");
};
--------------------------------------
【2.实例作为原型对象的继承】
缺点：就是被继承的类的属性有对象的时候，比如下面的arr,被继承的时候，是把属性的对象地址继承过去。实例化的时候，实例们都是引用的同一个地址。所以修改一个实例的该属性值，其他实例也会变化。
function P(argument) {
  this.name = "p";
  this.arr = [1,2,3];
}
P.prototype.p_fn = function() {
  console.log("p_fn");
};

function S () {
}
S.prototype= new P();

var sl_1 = new S();
var sl_2 = new S();
sl_1.arr.push("a");
console.log(sl_2.arr);  //[1,2,3,"a"]
--------------------------------------
【3.call+prototype】解决上面问题1+2
function P(argument) {
  this.name = "p";
}
P.prototype.p_fn = function() {
  console.log("p_fn");
};

function s_1 () {
  P.call(this);
}
s_1.prototype= P.prototype;
s_1.prototype.constructor = s_1;

这样写的缺点：因为父子的原型对象引用是同一个地址，修改s_1.prototype.constructor上的属性，那么父级上的这个属性也就被修改了。就没法区别被继承类的实例对象的构造函数是谁了。


【4.call+Object.create(prototype)】解决上面问题3
解决：挂这个中间层，在中间层新的对象上重新开constructor的属性。这样就完美解决。
s_1.prototype= Object.creat(P.prototype);
s_1.prototype.constructor = s_1;
```

* ES6继承
```js 
 class PeoPle(){
   constructor(name){
     this.name=name
   }
   eat(){
    console.log(`${this.name}eat something`)
   }
 }

 //子类通过extends继承父类的数据
 class Student extends PeoPle(){
     constructor(name,number){
      super(name)
      this.number=number
     }

     sayHi(){
      console.log(`姓名${this.name}学号${this.number}`)
     }
 }

//老师子类
  class teacher extends PeoPle(){
     constructor(name,number){
      super(name)
      this.major=major
     }

     teach(){
      console.log(`姓名${this.name}学号${this.major}`)
     }
 }



 const xialuo =new Student('夏洛',1000)
 console.log(xialuo.name)
 console.log(xialuo.number)
 xialuo.eat()
 xialuo.sayHi()

 const wanglaoshi =new teacher('夏洛',1000)
 console.log(xialuo.wanglaoshi)
 console.log(xialuo.wanglaoshi)
 wanglaoshi.eat()
 wanglaoshi.teach()
```


### 解构赋值

* 解构的变量需要和后面数据的key进行对应
* 需要对解构前，被解构的对象有key:val；
* 目的就是快速声明一个解构形式，把变量进行赋值；
```
let node = {
    type: "Identifier",
    name: "foo"
};
const { type, name } = node;
type  = 1;
```

* 提前赋值的话，需要进行小括号包裹进行赋值；
```
let node = {
    type: "Identifier",
    name: "foo"
},
type = "Literal",
name = 5;
单独赋值：就是前面没有对内部变量类型的声明；
({ type, name } = node);

console.log(type); // "Identifier"
console.log(name); // "foo"
```

* 解构赋值既是对变量赋值，又是返回一个对象，可以作为参数传入。
```
let node = {
    type: "Identifier",
    name: "foo"
};
function outputInfo(value) {
    console.log(value === node); // true
}
var { type, name } = {};
outputInfo({ type, name } = node);
```

* 也可以设置默认值
```
var {time=12,id=0}={};
console.log(time); // 12
```

* 多层嵌套就是看你解构到哪层
```
let obj = {
  a: {
    aa: 'aa',
    bb: 'bb'
  }
};
let { a } = obj;
console.log(a);  // {aa: 'aa',bb: 'bb'}
```

* 数组解构，就是通过位置标识来确认要解构的值是在哪里
```
let colors = [ "red", "green", "blue" ];
let [ , , thirdColor ] = colors;
console.log(thirdColor); // "blue"
```

* 如果有对应的变量声明，那就按照变量进行找
```
let a = 1,
    b = 2;
[ a, b ] = [ b, a ];
console.log(a); // 2
console.log(b); // 1
```

* 同样，数组嵌套的话，也是看解构到哪里了。
```
let colors = [ "red", [ "green", "lightgreen" ], "blue" ];
let [ firstColor, [ secondColor ] ] = colors;

console.log(firstColor); // "red"
console.log(secondColor); // "green"
```

### Map

* map对象上有多个方法，极其方便的操作数据
```
对象保存键值对，并且能够记住键的原始插入顺序。任何值（对象或者基本类型）都可以作为一个键或一个值

var map = new Map();
map.set('one', 1);
----------------------------------------------
var map = new Map([['one',1], ['two', 2], ['three', 3]]);
for(var name of map){
    console.log(name);
}
one,1
two,2
three,3
----------------------------------------------
console.log(map.size);  3  
map.clear();
console.log(map.size);  0
----------------------------------------------
console.log(map.has("one")); //true
map.delete("one");
console.log(map.has("one")); //false
----------------------------------------------
for(var [a,b] of map.entries()){
    console.log(a,b);
}

----------------------------------------------
map.forEach(function(value, key, mapObj) {
    console.log(value + '---' + key + '---' + mapObj);
    //value - Map对象里每一个键值对的值
    //key - Map对象里每一个键值对的键
    //mapObj - Map对象本身
    console.log(this); //this === window
});
map.forEach(function(value, key, mapObj) {
    console.log(value + '---' + key + '---' + mapObj);
    console.log(this);    //this === map
}, map)
----------------------------------------------
map.get(1); //'one'

for(var name of map.keys()){
    console.log(name);
}

for(var val of map.values()){
    console.log(val);
}
```

### 箭头函数

* 简化函数
```
var f = () => 5;
==
var f = function() { return 5};
-------------------------------------------------
【有参数】
var sum = ( a, b) => a + b;
==
var sum = function( a, b) {
    return a +b;
}
-------------------------------------------------
多行代码，需要用{}包起来
var sum = (a, b) => { 
    console.log(a,b);
    return a+b;
}
-------------------------------------------------
返回对象
var get_obj = id => ({id: id, anme: "Temp"});
```

* 用处：
```
[1,2,3].map(function(x){
    return x*x;
});
[1,2,3].map(x => x*x);
--------------------------------------------
var arr = [];
[1, 2, 3].map(x => arr.push(x * x));

--------------------------------------------
配合解构赋值，把变量进行数组话
const numbers = (...nums) => nums; 
numbers(1,2,3,4,5,6,7,8,9);  //[1,2,3,4,5,6,7,8,9]
```

### this指向问题

* 涉及到箭头函数在哪里的问题，箭头函数内部的this,永远为定义箭头时，所在函数的this.任何方法改变不了。
* 这个this指person
```
let person = {
  name: 'jike',
  init: function() {
    console.log(this);  // 调用时的对象(函数)
    document.body.onclick = () => {
      alert(this.name);               
    }
  }
}
person.init();  //this-->person对象
person.init.call(this);  //this-->windows
--------------------------------------------------------
var person = {
    name:'jike',
    init:()=>{
        console.log(this); // 创建时所在函数的this，就是window
        document.body.onclick = ()=>{
            alert(this.name);                  
        }
    }
}
person.init();  //this-->window
person.init.call({});  //this-->window
--------------------------------------------------------
function foo() {
  console.log(this);  //外面函数的this,直接调用就是widow
  setTimeout(() => {
    console.log('id:', this.id); //创建时的this,就是外面函数的this
  }, 100);
}

var id = 21;
foo.call({ id: 42 });  // 被指向其他对象，所以打印是{ id: 42 }；打印为42
-------------------------------------------------------
function foo() {
  setTimeout(function() {
    console.log('id:', this.id);  // 就是调用setTimeout函数的this,就是window
  }, 100);
}

var id = 21;
foo.call({ id: 42 });  
//虽然foo的this发生改变，但是setTimeout的this还是window；var id = 21;全局变量，打印为21
--------------------------------------------------------
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);  //this就是Timer对象
  // 普通函数
  setInterval(function () {  //this就是window
    this.s2++;
  }, 1000);
}
var timer = new Timer();
setTimeout(() => console.log('s1: ', timer.s1), 3100);  //3
setTimeout(() => console.log('s2: ', timer.s2), 3100);  //0

3s后
**
setInterval(() => this.s1++, 1000); 已经执行三次，this为timer对象
**
setInterval(function () {  this.s2++;}, 1000); 尽管执行三次，但是window上没有这个值。也就是说，setInterval改变的是window上s2的值，不是timer的s2，所以打印是最后打印的是timer对象上的s2值。
```

* 到这，知道function的this就是谁调用它，就是谁。()=>{}就是一直往上找所在函数的this;
* 除了this，以下三个变量在箭头函数之中也是不存在的，指向外层函数的对应变量：arguments、super、new.target。
* 由于箭头函数没有自己的this，所以当然也就不能用call()、apply()、bind()这些方法去改变this的指向。
* 箭头函数没有原型属性，所以就不能做构造函数。



