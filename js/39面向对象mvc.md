# Lesson 39 面向对象 



## 一、预览：

1. 面向对象 && ***MVC***
2. ***this***
3. ***new***  :[知乎链接](https://zhuanlan.zhihu.com/p/23987456)

1. [程序员也不知道什么是面向对象](https://www.zhihu.com/question/19854505)

2. [MDN 给了我们一堆术语](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript)



 MDN 里：

> ***Class*** 类                            定义对象的特征。它是对象的属性和方法的模板定义.
> ***Object*** 对象                      类的一个实例。
> ***Property*** 属性                  对象的特征，比如颜色。
> ***Method*** 方法                    对象的能力，比如行走。
> ***Constructor*** 构造函数     对象初始化的瞬间, 被调用的方法. 通常它的名字与包含它的类一致.
> ***Inheritance*** 继承              一个类可以继承另一个类的特征。
> ***Encapsulation*** 封装         一种把数据和相关的方法绑定在一起使用的方法.
> ***Abstraction*** 抽象             结合复杂的继承，方法，属性的对象能够模拟现实的模型。
> ***Polymorphism*** 多态        多意为‘许多’，态意为‘形态’。不同类可以定义相同的方法或属性。

---



## 二、封装  Model View Controller

**面向对象** ： **核心**  刷出了一个对象 ，这个对象有`你要的和核心` 和 `自定义的方法`



*[完整代码](https://github.com/FrankFang/resume-15-8)*

 	之前resume 项目中简化代码为 模块化。 其中均为 M V C 对象，可封装该三个**公共属性** 为**原型**

**:**节省内存。  `./js/base/View.js` 【Model.js  Controller.js】



### 1. VIew.js

```javascript
//引用： var view = View('.xxx')

window.View = function(selector){
    return document.querySelector(selector)
}
```



### 2. Model.js

``` javascript
// 引用： var model = Model({resourceName: '表名'})
//  

window.Model = function(options){
    let resourceName = options.resourceName
    return {
        init: function(){
            var APP_ID = 'xxx'
            var APP_KEY = 'sss'
            AV.init({appId: APP_ID, appKey: APP_KEY})
        },
        fetch: function(){
            var query = new AV.Query(resourceName)
            return query.find()
        },
        save： function(object){
            var x = AV.Object.extend(resourceName)
            var x = new X()
            return x.save(object)
        }
    }
}
```



### 3. Controller.js

``` javascript
/*
Controller({
    init:(view,model){
        this.xxx()
        this.yyy()
    },
    xxx(){}
    yyy(){}
})
*/

window.Controller = function(options){
    var init = options.init

    let object= {
        view:null,
        model:null,
        init: function(view,model){
            this.view = view
            this.model = model
            this.model.init()
            init.call(this,view,model)
            options.bindEvents.call(this)
        }
    }
    for(let key in options){
        if(key !== 'init'){
            object[key] = options[key]
        }
    }
    return object
}

// 引用：
var controller = Controller({
    init: function(view,model){
        this.messageList = view.querySelector('#messageList')
        this.form = view.querySelector('form')
        this.loadMessages()
    },
    loadMessages:function(){...}
    ,bindEvents: function(){...}
})
```



## 三、复习this

1.  this 是 ***.call()*** 第一个参数 。
2. 没有则去看 **该API的 源码**   ---》 找不到
3. 搜 MDN  + API



### 栗子：onclick addEventListener  $.on

``` javascript
// 以下可以用 .call(???)  强制指定this 为 ???

buttton.onclick = function(){  
    console.log(this)  //结果1
}

button.addEventListener('click',function(){
     console.log(this)  //结果2
})

$('ul').on('click','li',function(){
     console.log(this)  //结果3
})

//结果1  this 值为 触发事件的元素  =》button

//结果2  this 值为 该元素的引用 =》button

//结果3  this 值为	代理事件时： 代表与 selector 相匹配的元素 => li 元素
	// 直接事件时 代表 绑定事件的元素 =》 ul
```



#### 栗子2

``` javascript
function X(){
    return object = {
        name: 'object',
        f1(x){ x.f2()},
        f2(){ console.log(this) } //A 
    }}

var options = {
    name: 'options',
    f1(){},
    f2(){ console.log(this)} //B   
}

var x = X()
x.f1(options)

//B options
EX 2 :
function X(){
       return object = {
        name: 'object',
        f1(x){ x.f2().call(this) },
        f2(){ console.log(this) } //C
    } }
var options = {
    name: 'options',
    f1(){},
    f2(){ console.log(this)} //B   
}

var x = X()
x.f1(options)

//D object
EX 3:
function X(){
       return object = {
        name: 'object',
        options: null,
        f1(x){ this.options = x; this.f2()},
        f2(){ this.options.f2.call(this) }} }
var options = {
    name: 'options',
    f1(){},
    f2(){ console.log(this)} //this 是？ 
}

var x = X()
x.f1(options)

//object
```



## 四、new 是啥？



 Q : 制造100个兵

 A:  公共的属性放一个地方  与 士兵相关联  ==》 利用原型

``` javascript
function 士兵(id){
    //1. var temp = {}    
    
    //2. this = temp    
    //   士兵.prototype = { construcotr: 士兵}
    
    //3. this.__proto__ = 士兵.prototype
    this.ID = id // ID不能重复
    this.生命值 = 42 //自有属性
    /* 实际工作中不要这样写，因为 __proto__ 不是标准属性 */
    
    //4. return this
    
    // new就是 语法糖  贴心 帮我们省略这么多步骤
}

// 士兵.prototype.兵种 = "美国大兵"   法2

士兵.prototype = {					//法1
    //共有属性
    constructor: 士兵,   //由谁创造的：constructor 
    兵种:"美国大兵",
    攻击力：5,
    行走:function(){/* 行走的代码*/}，
    攻击:function(){/* 攻击的代码*/}，
    死亡:function(){/* 死亡的代码*/}
    //...
}

第一种直接覆盖 Constructor   上面法1
第二种 单独添加属性        上面法2
```

### JS 之父 创建了 new 关键字，可以少些 四行代码

> 1 我帮你创建临时对象   `var 临时对象 = {} `
>
> 2 我帮你绑定原型	`临时对象.__proto__ = 士兵.原型`
>
> 3 我帮你 return	  ` return 临时对象` 
>
> 4 原型统一叫做 prototype	 `士兵.原型`



### 可以判断：

``` javascript

var object = new Object()
//自有属性 空
object.proto === Object.prototype

var array = new Array('a','b','c')
//自有属性 0:'a' 1:'b' 2:'c',length:3

array.proto === Array.prototype
Array.prototype.proto === Object.prototype


var fn = new Function('x', 'y', 'return x+y')
//自有属性 length:2, 不可见的函数体: 'return x+y'

fn.proto === Function.prototype


======->
Array is a function
Array = function(){...}
Array.proto === Function.prototype

```

