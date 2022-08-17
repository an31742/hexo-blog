---
title: 前端面试之css3
date: 2022-07-27 22:28:46
tags: 前端面试
---

### CSS盒模型

#### 标准模型+IE模型
* 标准模型：宽高（width，height）=文本宽高   没有padding，border；
* IE模型：宽高（width，height）=文本宽高+padding+border；

#### 设置盒模型
* 标准模型：box-sizing:content-box; 默认
* IE模型：box-sizing:border-box;

#### 边距重叠

* 相邻元素的margin-top和margin-bottom会发生重叠
* 空内容的p标签也会发生重叠

* 兄弟：上下DOM的20px会重叠
```
  >.box {
    width: 100px;
    height: 100px;
    background-color: #fff;
    margin-bottom: 20px;
  }
  >.box_1 {
    margin-top: 20px;
    width: 100px;
    height: 100px;
    background-color: #100;
  }
```

* 父子关系，子DOM的10px超过父DOM出去。
```
  >.box_2 {  【父DOM没有高度】
    width: 100%;
    background-color: #444;
    >.son{
      width: 200px;
      height: 200px;
      margin-top: 10px;  【会超出去】
      background-color: #fff;
    }
  }
```

* 无论是本身的DOM的margin，还是超出的父DOM的margin。遇见了就会重叠
```
  >.box_1 {
    margin-top: 20px;
    width: 100px;
    height: 100px;
    background-color: #100;
    margin-bottom: 10px;  【本身的】
  }
  >.box_2 {
    width: 100%;
    background-color: #444;
    >.son{
      width: 200px;
      height: 200px;
      margin-top: 10px;  【超出去的】
      background-color: #fff;
    }
  }
```

#### margin负值得问题

* margin的top和left设置负值元素向上向左移动
* margin-right设置负值右侧元素左移，自身不受影响
* margin-bottom设置负值下侧元素上移，自身不受影响


#### BFC：块级格式化上下文  Block Fromatting Context


* 一块独立渲染区域，内部元素的渲染不会影响边界以外的元素

* 解决：
```
1.BFC区域的边距在垂直方向上不发生重叠
2.BFC的区域不会与浮动元素的边距重叠
3.BFC在页面上是独立的容器，里面外面的元素不会相互影响
4.计算父级BFC元素高度的时候，子元素为浮动元素也会参与计算。
```

#### 如何创建BFC

* 给发生重叠的DOM，加一个父级DOM，设置BFC
```
float的值不为none
position的值不为static或者relative
display的值为 table-cell, table-caption, inline-block, flex, 或者 inline-flex中的其中一个
overflow的值不为visible
```
* 推荐使用：overflow：hidden

#### BFC使用场景


* 清除浮动

* 修改上面的情况：涉及到规则的1/3/4
* 左右布局：规则2。FBC区域与外面的浮动元素,不会与浮动元素的边距重叠，。
```
<div class="box_4">
  <div class="left"></div>
  <div class="right"></div>
</div>
  >.box_4{
    width: 100%;
    background-color: #777;
    >.left{
      float: left;
      width: 100px;
      height: 100px;
      background-color: red;
    }
    >.right{
      height: 200px;
      background-color: #fff;
      overflow: hidden;  【FBC区域与外面的浮动元素。不会与浮动元素重叠】
    }
  }
```
* 父级内部有浮动元素，父级高度就不会撑起来，子DOM的margin也会撑出去，父级DOM想撑起来或不想被超出去，父级设置BFC。见box_5

#### 理解
* 如果说任何有上面情况的margin的DOM，怎么创建BFC？给当前DOM新增个父级，父级设置BFC。设置BFC的话，父级就成为块级格式化上下文。然后父级的DOM的高度计算就是把子DOM的margin全部算在内，外面相互之间也不再会发生影响。

#### 如何理解html语义化

* 让人更容易读懂  （增加代码可读性）
* 让搜索引擎更容易读懂  （增加seo）


#### float布局

* 使用float布局
* 两侧使用margin负值，以便中间内容横向重叠
* 防止中间内容被覆盖，一个用padding一个margin

#### absolute和relative依据什么定位

* relative依据自身定位
* absolute依据最近一层定位元素定位 如果没有任何定位元素依据body定位

#### line-height如何继承的

* 固定值或者比例直接继承，如果是是百分比，先计算再继承 line-height*font-size

#### rem是什么
* 是相对于根元素的长度单位   html的font-size