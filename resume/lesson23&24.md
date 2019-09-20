`git commit --amend -v` 修复上一次的commit【将这次的覆盖上次的】

---

## HTML 部分：

​	**a** 标签 **默认动作**： 要么 跳转 要么 锚点 【用：`a href="javascript:;"`可不进行操作】



---

## CSS 部分：

> 预览： 绝对定位居中方式  伪元素实现loading css 、border。 动画过渡效果 动画



### 1 .绝对定位元素 水平垂直居中 

>  *绝对定位小技巧：top:100% [而不是bottom:0]*

``` css
  position: absolute;
  top:0;
  right:0;
  bottom:0;
  left:0;
  margin:auto;
```



### 2. 动画  引用及延时动画

``` css
@keyframes name {
    0% { }
    100% { }
}
//引用：
div {
    animation: name 10s linear infinite;
}
animation-delay:1s;

//过渡 & 变形：要有个起始量为基础 + 类名切换/:hover激活/事件激活过渡生效
{
    transform: rotate(180deg); // :translate(-100%,0);
    // transition: width 2s, height 2s, transform 2s;
    transition:all .5s
}

```



### 3. flex 居中 / 整屏灰色背景

``` css
{
    position: fixed; //要定位
    top:0;
    left:0;
    display: flex;
    width: 100%;
    height: 100%;
    z-index: 1;
    justify-content: center;
    align-items: center;
    background:#888;
}
```

### ...

4. 盒子 四边**边框的宽度**【可以实现文字上下或者左右的偏移】`border-width`

   

5.  **阴影效果**`box-shadow:0 0 5px rgb(0,0,0,0.5)`

   

6. `nav>ul>li>a:hover::after` a hover时　展示它的伪类效果

​      `nav>ul>li:hover a::after` li hover 时 它的**所有**子元素a 展示它的伪类效果



7. **不换行** `white-space: nowrap;`  [会撑宽该标签]

---



## JS 部分：

 js 给元素*添加/移除* **类名**来切换 动画/其他特效  。 效果由 CSS 来实现 即：

> js 控制类名 来决定是否展示效果  css 实现具体效果



### **通过切换父元素的active 来 控制子元素的效果**  

> *往往使js 代码更简洁 逻辑更清晰*

``` css
例如：
nav>ul>father.active .son {  //灵活结合了选择器 来让该样式是否生效
    						//这样往往代码简单
    display: block;
}
.son {
    display: none;
}
// js 控制添加父元素的acive 类名就能控制 子元素的特效是否会实现 

```



### 1. 添加删除 类名操作

``` javascript
//如 元素ID 为 siteWelcome
siteWelcome.classList.add('active');
siteWelcome.classList.remove('active');
```



### 2. 滚动：

从文档顶部开始滚动过的像素值

`window.scrollY`  // 相当于当前所在 Y 轴 坐标

`element.offsetTop` // 元素在document上 Y轴方向 坐标

滚动到x,y轴位置：`window.scrollTo(x,y-num)`



### 3. 查询器 ：`querySelector` `querySelectorAll`

``` javascript
document.querySelectorAll('nav>ul>li.menuTigger'); //找所有含选择器的元素 --》是个集合
document.querySelector('li.menuTigger'); //找第一个含选择器的元素 -->只返回第一个元素 
```

#### **找含有某属性的元素**：

``` javascript
document.querySelector("[href='#"+closestTag.id+"']")
// '[href='#tagname.id"]'
```



### 4. 事件 ：`e.target`&`e.currentTarget`  

``` javascript
前者是 指 触发事件的那个元素  --》 触发的可能是 监听的子元素 (监听的元素的一部分 如 'li>a' )
后者 指 事件监听的那个元素
```



### 5. 找元素/父、兄、子孙

#### 1. node.tagName&  `node.nextSibling` 

一个弟弟节点 `node.nextSibling`  不一定是元素节点[bug]

在HTML文档中, `node.tagName`会返回其大写形式 :元素的标签名

``` javascript
返回值: 1 .空白符文本节点 [值 3]. 回车等可能被处理为文本节点。
       2. 元素节点 [值 1] 。 
       3. null [没有弟弟了]
//  用递归解决该API bug：找弟弟中的第一个元素节点 
let brother = a.nextSibling
while(brother.nodeType === 3){ 
//或者其他判断条件 比如 brother.tagName !== 'UL' 找元素名为ul的节点
    brother = brother.nextSibling
}
```



#### 2. 父/子孙 用node。

ele.`parentElement`  ele.`parentNode`

> parentElement 和 parentNode一样,
>
> 但parentNode是W3C标准的parentElement 只在IE中可用.

通过父亲找儿子： `navTag.parentNode.children` [集合是动态更新的]

通过父亲找父亲的兄弟： `navTag.parentNode.parentNode.children`



### 6. 阻止 a 标签的默认动作 ： `x.preventDefault()`



### 7 获取 a 标签元素 的属性 href 值 ：

``` javascript
a.getAttribute('href')  //如 #siteAbout
x.currentTarget.href   //http://xxx/#siteAbout
第一个　用户写的是什么就是什么　不作任何处理　
第二个　是带 http 协议的　浏览器添加上去的	
```



### 8 调试代码 `debugger`

调试代码 获取局部变量：`debugger`　获取**局部变量**的信息。

```javascript
xxxx 局部变量代码 element xxx
debugger  //需要断点的地方
console.log(element)
-----------在控制台即可打印关于局部变量的信息
```



---

## lesson 24 ：

预览:

> 1. 定时器的灵活使用 每秒移动距离 【引出👉】2. 缓动函数使动画效果不僵硬   3. 框架的使用方式 。
> 2. 动画实现方式： 1. JS 实现   2. animation   3.transform + transition



#### 1. Math.abs()  绝对值

#### 2. 元素.样式.属性

``` javascript
div1.style.position="relative" 
div1.style.left = '20px'  // 须带px
```

#### 3. 定时器  ---》 动画的原理 帧

`setTimeout()`   ` setInterval() ` **清除定时器：** `clearInterval(id)`

``` javascript
div1.style.positon = 'relative'
div.style.left= 0
var n = 0
var i =425
var id = setInter(()=>{
    if(n===i){
        window.clearInterval(id)
        return //防止n=n+1继续下去bug
    }
    n = n + 1
    div1.style.left = n + 'px'
   },40)
```

滚动到a+动画x：

``` javascript
// 简易版         S = v * t
var currentTop = window.scrollY  //当前所在y 轴位置
var targetTop = aTag.offsetTop //目标元素位置
var T = 1000  //周期
var n = 25   // 一共滚多少次  1秒25帧
var t = T / n  //每次滚 所花多少秒
var S = targetTop - currentTop  //所滚动的总路程 S 
var s = S / n     //每次滚动的单位路程 s
var i = 0		//计数 当前滚动次数
var id = setInterval(function(){   //动画
    if( i ===n ){       //25次清除计数器
        window.clearInterval(id)   
        return
    }
    i += 1
    window.scrollTo(0, currentTop + s * i)  //每次滚动的位置Y
},t)  //每隔多少秒滚一次

```



#### 4. 缓动函数 / 缓动框架Tweenjs

s = v * t   [**s** = *|div.offset - window.scrollY|*  **v[API**] =》Tweenjs 几秒内完成动画**t** =*s* * t' [t' =>每像素走多少秒]=> ]

``` javascript
var coords = { y: currentP }; // Start at (0, 0) 滚动Y坐标起始点
var tween = new TWEEN.Tween(coords) // Create a new tween that modifies 'coords'.
.to({y: targetP}, duration) // Move to (0, 200) in 1 second.到达目标位置&所花时间
.easing(TWEEN.Easing.Quadratic.InOut) // Use an 缓动API to make the animation 
.onUpdate(function () { // 完成动画的时间内每次的 Y 坐标:cords.y. 
                  window.scrollTo(0, coords.y);  //进行滚动
                    })
.start(); // Start the tween immediately.
            }
```



#### 5. 滚动时离哪个元素近

>  滚动高亮导航/ 元素出场动画 /  均与 滚动时屏幕顶部离哪个元素近有关

 通过标记 [如加属性] 需要监控的元素 ，实时监听滚动事件==》 获得滚动 y 坐标

判断监控的哪个元素离 y坐标最近 ，则添加 相应效果

**选择排序法：** 选第一个元素为最近的 与之后每个元素相比，距离最近则为最近的元素

``` javascript
let divTags = document.querySelectorAll('[data-ele]');    // 所有目标监控元素
let closestTag = document.querySelectorAll('[data-ele]')[0]; 	//取第一个元素为最近
let minVal = 0;  //标记每次最小的标签key
or (let i =1; i < divTags.length; i++) {    //遍历从第二个元素开始比较
  if((Math.abs(divTags[i].offsetTop-window.scrollY))  //比较元素.y - scrollY 的绝对值
      <(Math.abs(closestTag.offsetTop-window.scrollY))){
             minVal = i;
               closestTag =divTags[i]; //若不是最近 则把它赋给最近这个变量
                        }
                    }
//就得到了最近 元素 closestTag
```

