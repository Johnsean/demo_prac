# Lesson 36 MVC的 VC

1 . ***resume*** 加入 轮播

> 不要自己写，网上利用框架 / 已经完美的方案来解决
>
> **CRM**    --**C** :*copy  cdn* --**R** :*run* 打印 *obj* 说明能运行 --**M** 框架 ----*copy html/css/js -->modify them* 调试

2 . 引入MVC 思想

>**M 模块化 :** 体现在各个位置 =》 *html /css/js/ js*及**js内**的的规整 简明/  
>
>script js代码 功能单一化 代码=》少=》各单个文件**[**文件名一目了然**]** 再引入
>
>**V :** *html* 各模块单独 **id** [div标签]包起来 =>js内 *view =queryxxx 。*
>
>**C:** 针对 **V** 的操作 *V.xxx*  化为对象【*controller*】 的操作 传入参数 *View* 对象

准备 ： 1. 搜索插件 2 . 轮播的主体 作品-》截图    3 工具： unsplash.com / 截图 4：3/1:1比例

---



## 插件加入resume 步骤

### 使用

-直接使用 **jq** **插件** 轮播 如 ***swiper*** :

> -引入一个 ***.css***  一个***main.js***
>
> -引入后 直接**打印 Swiper**  成功说明可以使用了
>
> 在开始使用 。粘贴html 等   --html放 body第一行，然后调试 



### **修改css** 

> 将 **main.css** 引入放在 插件的 **.css** 之后引入就能**覆盖**了





## MVC

-MVC 前提 是  你是**模块化**的 。

**MVC 解决 代码一写就忘的问题 【时间长了再看，不知道写的什么】**



### **M**：1. 分=》文件 

> 代码乱？因为代码多 。代码不乱? 把代码变少=》 封装/ 分模块，化为各单文件吗、，引入
>
> -- 放入`.js 取名字`    工具函数注释`/* helper */` 

``` html
<script src="https://cdn.xxx1"></script>
<script src="https://cdn.xxx2"></script>

<!-- 模块化 -->
<script src="./js"></script>
<script src="https://cdn.xxx2"></script>
<script src="https://cdn.xxx2"></script>
<script src="https://cdn.xxx2"></script>
```





### 2. 立即执行函数 [全局变量互相骚扰]

> 解决全局变量互相骚扰的问题 只有不使用全局变量。 **ES5**中 局部变量只能利用函数实现。
>
> 立即执行函数 只推荐使用  `!function(){..}.call() `   。不是匿名的函数则函数变量名为全局变量。所以函数必须匿名。  / 括号法不推荐 形如 `xxx回车()()    ` 前面不小心多打了xxx  则也是声明了全局变量xxx
>
> ``` javascript
> // 多余的注释： 针对上面
> 1. 不想要全局变量
> 2. 要使用局部变量
> 3. ES 5 里面，只有函数有局部变量
> 4. 于是声明一个 function xxx. 然后 xxx.call()
> 5. 这个时候 xxx 是全局变量（全局函数）
> 6. 所以我们不能给这个函数名字
> 7. function(){}.call()
> 8. 但是Chrome 报错，语法错误
> 9. 试出来一种方法不报错:  ！/+/-function(){}.call() 我们不在乎这个函数的返回值
>    b.  (function(){}).call() 不推荐  
>    		xxx
>          (function(){}).call()   // 就变成了  xxx() 
>    c.	frank12313123123.call() 随机数函数名  不推荐
> ```
>
> ---



#### 若 **两模块需要交流** 则利用`window` 实现 

#### 1.直接暴露 

> ``` javascript
> //module-1.js 
> !function(){
>     var person = window.person = {
>         name: 'frank'
>     }    
> }.call()
> 
> //module-2.js
> !function(){
>     var person = window.person
>     console.log(person)
> }.call()
> ```

#### 2.闭包【不直接暴露】  

**匿名**函数+闭包+全局变量  得到的对象细节封装方法。不是直接操作变量。**安全**

> ```javascript
> !function(){
>     var person = {name:'frank',age:18}
>     window.frankGrowUp = function(){
>         person.age += 1
>         return person.age
>     }
> }.call()
> 
> !function(){
>     var newAge = window.frankGrowUp()
>     console.log(newAge)
> }.call()
> ```
>
> ``` javascript
> 1. 立即执行函数使得 person 无法被外部访问
> 2. 闭包使得匿名函数可以操作 person
> 3. window.frankGrowUp 保存了匿名函数的地址  //只有这条能修改
> 4. 任何地方都可以使用 window.frankGrowUp
> 以上推出=> 任何地方都可以使用 window.frankGrowUp 操作person ，
> 但是不能直接访问 person
> 
> 如： 修改第三条 👇👇👇  使用局部变量保存
> ```
>
> ``` javascript
> var accessor = function(){
>     var person = {name:'frank',age:18}
> 	
>     return function(){
>         person.age += 1
>         return person.age
>     }
> }
> var growUp = accessor.call()
> growUp.call()
> 
> window.a = function(){
>      var person = {name:'frank',age:18}
> 	
>     return function(){
>         person.age += 1
>         return person.age
>     }
> }.call()
> ```



### 3.改代码组织   js 模块些



## 什么是VC

### View : Html element

``` javascript
//html中 div 括起来   这就是view 用户看得见的东西
<div id="mySlides">...</div>

//demo1
var view = document.querySelector('#mySlides')
view.style.border = '1px solid red'  //能看到作用范围
var mySwiper = new Swiper(view.querySelector('.swiper-container'),{..})

//demo2
var view = document.querySelector('#topNavBar')
window.addEventListener('scroll',function(x){
    if(window.scrolly > 0){
        view.classList.add('sticky')
    }else{
        view.calssList.remove('sticky')
    }
}).call()

//demo3
var view = document.querySelector('nav.menu')
let aTags = view.querySelectorAll('nav.menu > ul')
function animate(time){...}
```



### Controller : controller(view)

``` javascript
//demo2 
!function(){ 
    var view = document.querySelector('#topNavBar')
    var controller = {
        view: null,
        init: function(view){
            var view = this.view
            window.addEventListener('scroll',function(x){
                if(window.scrolly > 0){
                    view.classList.add('sticky')
                }else{
                    view.calssList.remove('sticky')
                }
            })
        }
    }
	controller.init(view)
}.call()


//======> 变成对象--------行为就可以分开了： 还可以拆分 init成员

!function(){ 
    var view = document.querySelector('#topNavBar')
    var controller = {
        view: null,
        init: function(view){
            this.view = view
            this.bindEvents()      
            // this.bindEvents.call(this)  这个this通过init()得到 指init的this
        }
        bindEvents: function(){
            var view = this.view    //这个this 通过上面bind()得到 指上面的this
            window.addEventListener('scroll',function(x){
                if(window.scrolly > 0){
                    view.classList.add('sticky') 
                    //法2这里若写this 则指用户滚到的元素，随机。想确定它则用bind 
                    //法3箭头函数 不用bind(xxx)
                }else{
                    view.calssList.remove('sticky')
                }
            }//.bind(this) 法2绑定自己要的那个this 而不是随机 
               )
        }
    }
	controller.init(view)
	//controller.init.call(controller.view)   这个this 是controller
}.call()
```

#### 箭头函数`=>`    内外this不变

> 因为：没有 this 这时的this 像变量一样就近原则【值为最近的那个this的值】。
>
> ``` javascript
> this  //window
> var f = ()=>console.log(this) 
> f.call({name:'frank'}) //window   传入this进去 得到的倒是winow而不是{...}
> ```

``` javascript
// 法3箭头函数 不用 .bind(this)
bindEvents: function(){
    var view = this.view 
    window.addEventListener('scroll',(x)=>{
        if(window.scrolly > 0){
            this.active()
        }else{
            this.deactive()
        }
    })  //箭头函数没有 this
},
active:function(){
    this.view.classList.add('sticky')
    },
deactive:function(){
    this.view.classList.remove('sticky')
} 
...
```



#### 3个demo 对象化：

``` javascript
//demo2 对象化 ↑👆👆👆↑

//demo1
!function(){
    var view = document.querySelector('#mySlides')
    var controller = function(view){
        var mySwiper = new Swiper(view.querySelector('.swiper-container'),{loop:true,...})
        }
    controller(view)
}.call()

//====>>>>
!function(){
    var view = document.querySelector('#mySlides')
    var controller = {
        view: null,
        swiper: null,
        swiperOptions: {loop:true,...},
        init: function(view){
            this.view = view
            this.initSwiper()
        },
        initSwiper: function(){
            this.swiper = new Swiper(
                view.querySelector('.swiper-container'),
                this.swiperOptions
            )
        }
    }
    controller.init(view)
}.call()
    
    
//demo 3
    !function(){
        var view = document.querySelector('nav.menu')

        var controller = {
            view: null,
            aTags: null,
            init: function(view){
                this.view = view
                this.initAnimation()
                this.bindEvents()
            },
            initAnimation: function(){
                function animate(time){ 
                    requestAnimationFrame(animate);
                    TWWEEN.update(time);
                }
                requestAnimationFrame(animate);
            },
            scrollToElement: function(element){
                let top = element.offsetTop
                //...
            },
            bindEvents: function(){
                let aTags = view.querySelectorAll('nav.menu > ul')
                //...
                let element = document.querySelector(href)
                this.scrollToElement(element)
            }  
        }
        controller.init(view)
}.call()
```


