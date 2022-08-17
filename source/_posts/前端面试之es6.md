---
title: 前端面试之面试问题05
date: 2022-07-28 01:26:09
tags: 前端面试
---

# 面试总结



## 1，简单的介绍一下你自己

## 2，你都遇见过哪些困难

## 3，es6有哪些新特性

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

用于解决地狱回调，将异步转化为同步

```js
`let p = new Promise((resolve,reject) => {
    reject('error');
});

p.catch(result => {
    console.log(result);
})`

```

#### 3.5什么是async和await

async和await是将异步强行转换为同步处理。 

async/await与promise不存在谁代替谁的说法，因为async/await是寄生于Promise，Generater的语法糖。 

```js
async function demo() {
let result01 = await sleep(100);
//上一个await执行之后才会执行下一句
let result02 = await sleep(result01 + 100);
let result03 = await sleep(result02 + 100);
// console.log(result03);
return result03;
}

```

#### 3.6async和await有什么区别吗

1 promise是ES6，async/await是ES7
2 async/await相对于promise来讲，写法更加优雅
3 reject状态：
    1）promise错误可以通过catch来捕捉，建议尾部捕获错误，
    2）async/await既可以用.then又可以用try-catch捕捉

#### 3.7箭头函数和普通函数有什么区别

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

#### 3.8什么是rest参数

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

#### 3.9字符串的扩展

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

#### 3.10数组的扩展

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

3,Set 的成员

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
// 遍历Set对象，发现重复的值只有一份
// for...in  循环中的 i 表示数组的下标，或对象的属性名
// for...of  循环中的 i 表示数组的值，或对象的值
for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

## 4，数组去重的方法

1，**Methods 1**: 思路：定义一个新数组，并存放原数组的第一个元素，然后将元素组一一和新数组的元素对比，若不同则存放在新数组中。 

```js
function unique(arr){
 var res = [arr[0]];
 for(var i=1; i<arr.length; i++){
  var repeat = false;
  for(var j=0; j<res.length; j++){
   if(arr[i] === res[j]){
    repeat = true;
    break;
   }
  }
  if(!repeat){
   res.push(arr[i]);
  }
 }
 return res;
}
console.log('------------方法一---------------');
 
console.log(unique([1,1,2,3,5,3,1,5,6,7,4]));
```

2,**Methods 2:** 思路：先将原数组排序，在与相邻的进行比较，如果不同则存入新数组。 

```js
function unique2(arr){
    //先排序
 var arr2 = arr.sort();
    //然后获取
 var res = [arr2[0]];
 for(var i=1; i<arr2.length; i++){
  if(arr2[i] !== res[res.length-1]){
   res.push(arr2[i]);
  }
 } 
 return res;
}

console.log('------------方法二---------------');

console.log(unique2([1,1,2,3,5,3,1,5,6,7,4]));

```

3,**Methods 3**: 利用对象属性存在的特性，如果没有该属性则存入新数组。 

```js
function unique3(arr){
 var res = [];
 var obj = {};
 for(var i=0; i<arr.length; i++){
  if( !obj[arr[i]] ){
   obj[arr[i]] = 1;
   res.push(arr[i]);
  }
 } 
 return res;
}

console.log('------------方法三---------------');

console.log(unique3([1,1,2,3,5,3,1,5,6,7,4]));

```

4，**Methods 4:** 利用数组的indexOf下标属性来查询。 

```js
function unique4(arr){
    //设置一个空数组
 var res = [];
    //循环要去重的数组
 for(var i=0; i<arr.length; i++){
    //判断数组下标
     //indexOf方法可返回某个指定的字符串值在字符串中首次出现的位置。
  if(res.indexOf(arr[i]) == -1){
   res.push(arr[i]);
  }
 }
 return res;
}

console.log('------------方法四---------------');

console.log(unique4([1,1,2,3,5,3,1,5,6,7,4]));

```

5，**Methods 5**: 利用数组原型对象上的includes方法。 

```js
function unique5(arr){
    //这是一个空的数组
 var res = [];
    //循环遍历
 for(var i=0; i<arr.length; i++){
     //判断新数组是否包含这个数组如果不包含就添加
  if( !res.includes(arr[i]) ){ // 如果res新数组包含当前循环item
   res.push(arr[i]);
  }
 }
 return res;
}

console.log('------------方法五---------------');

console.log(unique5([1,1,2,3,5,3,1,5,6,7,4]));

```

6，**Methods 6**: 利用数组原型对象上的 filter 和 includes方法。 

```js
function unique6(arr){
 var res = [];
 
 res = arr.filter(function(item){
  return res.includes(item) ? '' : res.push(item);
 });
 return res;
}

console.log('------------方法六---------------');

console.log(unique6([1,1,2,3,5,3,1,5,6,7,4]));

```

7,**Methods 7**: 利用数组原型对象上的 forEach 和 includes方法。 

```js
function unique7(arr){
 var res = [];
 
 arr.forEach(function(item){
  res.includes(item) ? '' : res.push(item);
 }); 
 return res;
}

console.log('------------方法七---------------');

console.log(unique7([1,1,2,3,5,3,1,5,6,7,4]));

```

8,**Methods 8**: 利用数组原型对象上的 splice 方法。 

```js
function unique8(arr){
 var i,
  j,
  len = arr.length; 
 for(i = 0; i < len; i++){
  for(j = i + 1; j < len; j++){
   if(arr[i] == arr[j]){
    arr.splice(j,1);
    len--;
    j--;
   }
  }
 }
 return arr;
}

console.log('------------方法八---------------');

console.log(unique8([1,1,2,3,5,3,1,5,6,7,4]));


```

9,**Methods 9**: 利用数组原型对象上的 lastIndexOf 方法。 

```js
function unique9(arr){
 var res = []; 
 for(var i=0; i<arr.length; i++){
  res.lastIndexOf(arr[i]) !== -1 ? '' : res.push(arr[i]);
 }
 return res;
}

console.log('------------方法九---------------');

console.log(unique9([1,1,2,3,5,3,1,5,6,7,4]));


```

10,**Methods 10**: 利用 ES6的set 方法 

```
function unique10(arr){
 //Set数据结构，它类似于数组，其成员的值都是唯一的
 return Array.from(new Set(arr)); // 利用Array.from将Set结构转换成数组
}
 
console.log('------------方法十---------------');
 
console.log(unique10([1,1,2,3,5,3,1,5,6,7,4]));
```

## 5，什么是闭包，闭包都有哪些特性

闭包是什么： JS 的函数内部可以使用函数外部的变量

正是由于 JS 的函数内部可以使用函数外部的变量，所以这段代码正好符合了闭包的定义。而不是 JS 故意要使用闭包。 

闭包的特性：

1. 函数嵌套函数
2. 函数内部可以引用外部的参数和变量
3. 参数和变量不会被垃圾回收机制回收

**闭包会造成内存泄露？**

**错。**

**说这话的人根本不知道什么是内存泄露。内存泄露是指你用不到（访问不到）的变量，依然占居着内存空间，不能被再次利用起来。** 

**闭包里面的变量明明就是我们需要的变量（lives），凭什么说是内存泄露？**

**这个谣言是如何来的？**

**因为 IE。IE 有 bug，IE 在我们使用完闭包之后，依然回收不了闭包里面引用的变量。**

**这是 IE 的问题，不是闭包的问题。参见司徒正美的[这篇文章](https://link.zhihu.com/?target=http%3A//www.cnblogs.com/rubylouvre/p/3345294.html)。

闭包的优点：

可以设计私有的方法和变量 

闭包的作用：

闭包常常用来「间接访问一个变量」。换句话说，「隐藏一个变量」。 

这里面确实有闭包，local 变量和 bar 函数就组成了一个闭包（Closure）。

```js
function foo(){
  var local = 1
  function bar(){
    local++
    return local
  }
  return bar
}

var func = foo()
func()
//为什么要函数套函数呢

//是因为需要局部变量，所以才把 local 放在一个函数里，如果不把 local 放在一个函数里，local 就是一个全局变量了，达不到使用闭包的目的——隐藏变量（等会会讲）。 

```

## 6，什么是作用域，什么是作用域链

作用域：变量起作用的范围
        * js中只有两种：全局作用域  局部作用域
        *
       1.全局作用域：变量在任何地方起作用
        * 全局变量:在函数外面声明

​       2.局部作用域：变量只能在函数内部起作用
       局部变量：在函数内部声明

- 1.作用域链是怎么来的
  - 默认情况下，我们的js代码处于全局作用域，当我们声明一个函数时，此时函数体会开辟一个局部作用域， 如果我们在这个函数体中又声明一个函数，那么又会开辟一个新的局部作用域，以此类推，就会形成一个作用域链
- 2.变量在作用域链上的访问规则
  - 就近原则：访问变量时，会优先访问的是在自己作用域链上声明的变量，如果自己作用域链上没有声明这个变量，那么就往上一级去找有没有声明这个变量，如果有就访问，如果没有就继续往上找有没有声明，直到找到0级作用域链上，如果有，就访问，如果没有就报错

## 7，什么是构造函数

```js
function Person (name, age) {
  this.name = name
  this.age = age
  this.sayName = function () {
    console.log(this.name)
  }
}

var p1 = new Person('张三', 18)
p1.sayName() // => 张三

var p2 = new Person('李四', 23)
p2.sayName() // => 李四
```

在上面的示例中，Person() 函数取代了 createPerson() 函数，但是实现效果是一样的。 这是为什么呢？

我们注意到，Person() 中的代码与 createPerson() 有以下几点不同之处：

没有显示的创建对象

直接将属性和方法赋给了 this 对象

没有 return 语句

**函数名使用的是大写的 Person要创建 Person 实例，则必须使用 new 操作符。 以这种方式调用构造函数会经历以下 4 个步骤：**

**创建一个新对象。**

**将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）。**

**执行构造函数中的代码。**

**返回新对象。**

```js
function Person (name, age) {
  // 当使用 new 操作符调用 Person() 的时候，实际上这里会先创建一个对象
  // var instance = {}
  // 然后让内部的 this 指向 instance 对象
  // this = instance
  // 接下来所有针对 this 的操作实际上操作的就是 instance

  this.name = name
  this.age = age
  this.sayName = function () {
    console.log(this.name)
  }

  // 在函数的结尾处会将 this 返回，也就是 instance
  // return this
}
```

#### 构造函数与实例对象的关系

使用构造函数的好处不仅仅在于代码的简洁性，更重要的是我们可以识别对象的具体类型了。

 在每一个实例对象中的_*proto_*中同时有一个 `constructor` 属性，该属性指向创建该实例的构造函数： 

```js
console.log(p1.constructor === Person) // => true
console.log(p2.constructor === Person) // => true
console.log(p1.constructor === p2.constructor) // => true
```

对象的 `constructor` 属性最初是用来标识对象类型的， 但是，如果要检测对象的类型，还是使用 `instanceof` 操作符更可靠一些： 

```js
console.log(p1 instanceof Person) // => true
console.log(p2 instanceof Person) // => true
```

总结：

1 构造函数是根据具体的事物抽象出来的抽象模板。

2 实例对象是根据抽象的构造函数模板得到的具体实例对象。

3 每一个实例对象都具有一个 constructor 属性，指向创建该实例的构造函数。（ 此处constructor 是实例的属性的说法不严谨，具体后面的原型会讲到）

4 可以通过实例的 constructor 属性判断实例和构造函数之间的关系。（这种方式不严谨，推荐使用 instanceof 操作符，后面学原型会解释为什么）

#### **构造函数的问题**

```js
function Person (name, age) {
  this.name = name
  this.age = age
  this.type = '学生'
  this.sayHello = function () {
    console.log('hello ' + this.name)
  }
}

var p1 = new Person('王五', 18)
var p2 = new Person('李四', 16)
```

上边的代码，从表面看上好像没什么问题，但是实际上这样做，有一个很大的弊端。 那就是对于每一个实例对象，`type` 和 `sayHello` 都是一模一样的内容， 每一次生成一个实例，都必须为重复的内容，多占用一些内存，如果实例对象很多，会造成极大的内存浪费。 

```js
console.log(p1.sayHello === p2.sayHello) // => false
```

对于这种问题我们可以把需要共享的函数定义到构造函数外部： 

```js
function sayHello = function () {
  console.log('hello ' + this.name)
}

function Person (name, age) {
  this.name = name
  this.age = age
  this.type = '学生'
  this.sayHello = sayHello
}

var p1 = new Person('王五', 18)
var p2 = new Person('李四', 16)

console.log(p1.sayHello === p2.sayHello) // => true
```

这样确实可以了，但是如果有多个需要共享的函数的话就会造成全局命名空间冲突的问题。如何解决这个问题呢？你肯定想到了可以把多个函数放到一个对象中用来避免全局命名空间冲突的问题 

```js
var fns = {
  sayHello: function () {
    console.log('hello ' + this.name)
  },
  sayAge: function () {
    console.log(this.age)
  }
}

function Person (name, age) {
  this.name = name
  this.age = age
  this.type = '学生'
  this.sayHello = fns.sayHello
  this.sayAge = fns.sayAge
}

var p1 = new Person('王五', 18)
var p2 = new Person('李四', 16)

console.log(p1.sayHello === p2.sayHello) // => true
console.log(p1.sayAge === p2.sayAge) // => true
```



## 8，什么是原型，

**Javascript 规定，每一个构造函数都有一个 prototype 属性，指向另一个对象。 这个对象的所有属性和方法，都会被构造函数的实例继承** 

这也就意味着，我们可以把所有对象实例需要共享的属性和方法直接定义在 `prototype` 对象上。

```js
function Person (name, age) {
  this.name = name
  this.age = age
}

console.log(Person.prototype)

Person.prototype.type = '学生'

Person.prototype.sayName = function () {
  console.log(this.name)
}

var p1 = new Person(...)
var p2 = new Person(...)

console.log(p1.sayName === p2.sayName) // => true
```

这时所有实例的 **type 属性和 sayName() 方法**， 其实都是同一个内存地址，指向 `prototype` 对象，因此就提高了运行效率。 

**任何函数都有一个 prototype 属性，该属性是一个对象。** 

```js
function F () {}
console.log(F.prototype) // => object

F.prototype.sayHi = function () {
  console.log('hi!')
}
```

**构造函数的 prototype 对象默认都有一个 constructor 属性，指向 prototype 对象所在函数。** 

```js
console.log(F.constructor === F) // => true
```

**通过构造函数得到的实例对象内部会包含一个指向构造函数的 prototype 对象的指针 proto**

```js
var instance = new F()
console.log(instance.__proto__ === F.prototype) // => true
```

**`__proto__` 是非标准属性。**

实例对象可以直接访问原型对象成员：

```js
instance.sayHi() // => hi!
```

**总结：**

**任何函数都具有一个 prototype 属性，该属性是一个对象。**

**构造函数的 prototype 对象默认都有一个 constructor 属性，指向 prototype 对象所在函数。**

**通过构造函数得到的实例对象内部会包含一个指向构造函数的 prototype 对象的指针 proto。**

**所有实例都直接或间接继承了原型对象的成员。**

## 9，什么是原型链

**每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性。** 

1. 搜索首先从对象实例本身开始。
2. 如果在实例中找到了具有给定名字的属性，则返回该属性的值。
3. 如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。
4. 如果在原型对象中找到了这个属性，则返回该属性的值。

**也就是说，在我们调用 person1.sayName() 的时候，会先后执行两次搜索**：

首先，解析器会问：“实例 person1 有 sayName 属性吗？”答：“没有。

然后，它继续搜索，再问：“ person1 的原型有 sayName 属性吗？”答：“有。

于是，它就读取那个保存在原型对象中的函数。

当我们调用 person2.sayName() 时，将会重现相同的搜索过程，得到相同的结果。

这就是多个对象实例共享原型所保存的属性和方法的基本原理 

**总结：**

1. 先在自己身上找，找到即返回。
2. 自己身上找不到，则沿着原型链向上查找，找到即返回。
3. 如果一直到原型链的末端还没有找到，则返回 `undefined。`

#### 9.2 实例对象读写原型对象成员

读取：

**先在自己身上找，找到即返回。**

**自己身上找不到，则沿着原型链向上查找，找到即返回。**

**如果一直到原型链的末端还没有找到，则返回 undefined。**

值类型成员写入（实例对象.值类型成员 = xx）：

**当实例期望重写原型对象中的某个普通数据成员时实际上会把该成员添加到自己身上。**

**也就是说该行为实际上会屏蔽掉对原型对象成员的访问。**

引用类型成员写入（实例对象.引用类型成员 = xx）：同上。

复杂类型修改（实例对象.成员.xx = xx）：

**同样会先在自己身上找该成员，如果自己身上找到则直接修改。**

**如果自己身上找不到，则沿着原型链继续查找，如果找到则修改。**

**如果一直到原型链的末端还没有找到该成员，则报错（实例对象.undefined.xx = xx）**。

原型链的定义：

原型链是一种关系,是实例对象和原型对象之间的关系,这种关系是通过原型(proto)来联系的。JavaScript 对象有一个指向一个原型对象的链。当试图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾。

```js
   // 人的构造函数
    function Person(name,age) {
      // 属性
      this.name=name;
      this.age=age;
      // 在构造函数中的方法
      this.eat=function () {
        console.log("吃吃吃");
      };
    }
    // 添加共享的属性
    Person.prototype.sex="男";
    // 添加共享的方法
    Person.prototype.sayHi=function () {
      console.log("哈哈哈");
    };
    // 实例化对象,并初始化
    var per=new Person("张三",18);
    per.sayHi();
```

上边的代码中实例对象的原型__proto__和构造函数的原型prototype指向是相同的，实例对象中的__proto__原型指向的是构造函数中的原型prototype。 

```js
console.log(per.__proto__==Person.prototype);// true
```

原型链

![](D:\面试\简历\面试总结\原型链.png) {% asset_img 原型链.png %}

#### **9.3原型的指向**

```js
   //人的构造函数
    function Person(age) {
      this.age=10;
    }
    //人的原型对象方法
    Person.prototype.eat=function () {
      console.log("吃吃吃!!!");
    };
    //学生的构造函数
    function Student() {
 
    }
    Student.prototype.sayHi=function () {
      console.log("学学学，为中华崛起而读书!!!");
    };
    //学生的原型,指向了一个人的实例对象
    Student.prototype=new Person(18);
    var stu=new Student();
    console.dir(stu);
    stu.eat(); // 吃吃吃!!!
    stu.sayHi();//错误stu.sayHi is not a function，原型指向发生改变，找不到sayHi()方法
```

上边的代码中，实例对象stu调用原型方法sayHi()的时候发生错误，这是因为Student.prototype=new Person(18);使原型指向发生了改变，stu对象中已经找不到sayHi()方法。**原型指向改变分析： 图中的红线构成原型链。** 

在1.3.1的代码中，因为原型指向发生改变，找不到sayHi()方法而发生错误，我们可以在原型改变指向之后添加原型方法,改变代码如下： 

```js
    //人的构造函数
    function Person(age) {
      this.age=10;
    }
    //人的原型对象方法
    Person.prototype.eat=function () {
      console.log("吃吃吃!!!");
    };
    //学生的构造函数
    function Student() {
 
    }
    //学生的原型,指向了一个人的实例对象
    Student.prototype=new Person(18);
    Student.prototype.sayHi=function () {
          console.log("学学学，为中华崛起而读书!!!");
    };
    var stu=new Student();
    console.dir(stu);
    stu.eat(); // 吃吃吃!!!
    stu.sayHi();// 学学学，为中华崛起而读书!!!
```

**注意:**简单的原型写法，会将 Person.prototype 重置到了一个新的对象，即改变原型的指向。我们也应该在原型改变指向之后添加原型方法。 

```js
   function Person(age) {
      this.age = age;
    }
 
    //指向改变了
    Person.prototype = {
      eat: function () {
        console.log("吃");
      }
    };
    //添加原型方法
    Person.prototype.sayHi = function () {
      console.log("你好帅呀!!!");
    };
    var per = new Person(10);
    per.sayHi();// 你好帅呀!!!
```

9.4实例对象属性和原型对象属性重名

**实例对象访问某个属性,应该先从实例对象中找,找到了就直接用，找不到就去指向的原型对象中找,找到了就使用,找不到返回undefined。** 

```js
    function Person(age,sex) {
      this.age=age;
      this.sex=sex;
    }
    Person.prototype.sex="女";
    var per=new Person(18,"男");
    console.log(per.sex); // 男
    // 因为JS是一门动态类型的语言,如果对象没有这个属性,只要对象.属性名字,对象就有了这个属性了
    // 但是该属性没有赋值, 所以per.albert结果是:undefined
    console.log(per.albert); // undefined
    console.log(albert); // 报错
```

通过实例对象不能改变原型对象中的属性值，要想改变原型对象中的属性值,应该直接通过原型对象.属性=值；进行改变。 

```js
   Person.prototype.sex="我的天呐!!!我为什么这么帅?";
   per.sex="不知道";
   console.log(per.sex);
 
   console.dir(per);
```

## 10，继承

面向对象编程思想是根据需求分析对象，找到对象有什么特征和行为，然后通过代码的方式来实现需求。要想实现这个需求，就要创建对象，要想创建对象，就应该有构造函数，然后通过构造函数来创建对象，通过对象调用属性和方法来实现相应的功能及需求。
因为面向对象的思想适合于人的想法，编程起来会更加的方便，后期维护的时候也要会更加容易，所以我们才要学习面向对象编程。但JS不是一门面向对象的语言，而是一门基于对象的语言。JS不像JAVA,C#等面向对象的编程语言中有类(class)的概念(也是一种特殊的数据类型),JS中没有类(class),但是JS可以模拟面向对象的思想编程,在JS可以通过构造函数来模拟类的概念(class)。在ES6中，class (类)作为对象的模板被引入，可以通过 class 关键字定义类，但是它的的本质还是 function

#### 10.1什么是继承

继承是一种类(class)与类之间的关系,JS中没有类,但是可以通过构造函数模拟类,然后通过原型来实现继承，继承是为了实现数据共享，js中的继承当然也是为了实现数据共享。

继承是子类继承父类的特征和行为，使得子类对象（实例）具有父类的属性和方法，或子类从父类继承方法，使得子类具有父类相同的行为。继承可以使得子类具有父类的各种属性和方法，而不需要再次编写相同的代码。

#### 10.2通过原型实现继承

```js
// 人
function Person(name, age, sex) {
	this.name = name;
	this.sex = sex;
	this.age = age;
}
Person.prototype.eat = function() {
	console.log("吃吃吃!!!");
};
Person.prototype.sleep = function() {
	console.log("睡睡睡!!!");
};
 
 
//学生
function Student(score) {
	this.score = score;
}
//改变学生的原型的指向即可让学生继承人
Student.prototype = new Person("张三", 18, "男");
Student.prototype.study = function() {
	console.log("学习真的太累了！！！");
};
 
var stu = new Student(99);
console.log("学生从人中继承的属性和行为:"); // >学生从人中继承的属性和行为:
console.log(stu.name); // >张三
console.log(stu.age); // >18
console.log(stu.sex); // >男
stu.eat(); // >吃吃吃!!!
stu.sleep(); // >睡睡睡!!!
console.log("学生中自己有的属性和行为:"); // >学生中自己有的属性和行为:
console.log(stu.score); // >99
stu.study(); // >学习真的太累了！！！
```

#### 10,3通过构造函数实现继承

```js
// 动物的构造函数
function Animal(name, weight) {
	this.name = name;
	this.weight = weight;
}
//动物的原型的方法
Animal.prototype.eat = function() {
	console.log("弟兄们冲啊，赶快吃吃吃!!!");
};
 
//狗的构造函数
function Dog(color) {
	this.color = color;
}
Dog.prototype = new Animal("小三", "30kg");
Dog.prototype.bitePerson = function() {
	console.log("~汪汪汪~,快让开,我要咬人了!!!");
};
 
//哈士奇构造函数
function Husky(age) {
	this.age = age;
}
Husky.prototype = new Dog("黑白色");
Husky.prototype.playYou = function() {
	console.log("咬坏充电器,咬坏耳机,拆家...哈哈，好玩不!!!");
};
var husky = new Husky(3);
console.log(husky.name, husky.weight, husky.color);
husky.eat();
husky.bitePerson();
husky.playYou();
```

![](D:\面试\简历\面试总结\继承.png)



#### 10.4借用构造函数实现继承

在上边的讲解中，我们为了数据共享,改变了原型指向,做到了继承，即通过改变原型指向实现了继承。这导致了一个问题，因为我们改变原型指向的同时,直接初始化了属性，这样继承过来的属性的值都是一样的了。这是个问题，如果我们想要改变继承过来的值，只能重新调用对象的属性进行重新赋值，这又导致我们上边的初始化失去了意义。

```js
function Person(name, age, sex, weight) {
	this.name = name;
	this.age = age;
	this.sex = sex;
	this.weight = weight;
}
Person.prototype.sayHi = function() {
	console.log("你好帅呀!!!");
};
 
function Student(score) {
	this.score = score;
}
//希望人的类别中的数据可以共享给学生---继承
Student.prototype = new Person("小三", 18, "男", "58kg");
 
var stu1 = new Student("99");
console.log(stu1.name, stu1.age, stu1.sex, stu1.weight, stu1.score);
stu1.sayHi();
 
var stu2 = new Student("89");
console.log(stu2.name, stu2.age, stu2.sex, stu2.weight, stu2.score);
stu2.sayHi();
 
var stu3 = new Student("66");
console.log(stu3.name, stu3.age, stu3.sex, stu3.weight, stu3.score);
stu3.sayHi();
```

## https://blog.csdn.net/qq_23853743/article/details/108348436