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
