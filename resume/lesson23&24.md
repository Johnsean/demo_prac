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
    transform: rotate(180deg);
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

   5 . **阴影效果**`box-shadow:0 0 5px rgb(0,0,0,0.5)`

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



### 4. 事件 ：`e.target`&`e.currentTarget`  

``` javascript
前者是 指 触发事件的那个元素  --》 触发的可能是 监听的子元素 (监听的元素的一部分 如 'li>a' )
后者 指 事件监听的那个元素
```



### 5. node.tagName&  `node.nextSibling` 

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

