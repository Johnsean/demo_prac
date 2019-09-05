# Lessons.20-22

##  一、Js 历史

​	91年至XXX：  李爵士 www  ---  同事 CSS --- W3C   ===> *HTML+ CSS*

​				网景 *Netscape* =>**Navigator** 带脚本[Mocha抹茶]的浏览器 -- 》*LiveScript ->JavaScript* [商标]

​				*Sun*  Java   

​		----- > *Unicode* 发布-----> *UTF-8*   [在编码出来前 JS就发布了 所以用的旧编码格式 编码会有**Bug**]

​		-----》 MS IE => **Jscript**   导致 网景 **开源**--》*Firefox*  && 向 *ECMA* 申报行业标准 ---- **ECMAScript**

​		-------》**IE 5.5** MS 推出 **Js发请求**功能  ----直到*Gmail* 发布 网页上的程序------》JS被认可编程语言

​		

​	前端 Front-end (以JS为生 ) 

​			JS 不行（缺点） ：1 全局变量 (没有模块化 容易被覆盖等) 2.标准库（内置代码少）  

​			**ES 3** ==> /* ES4(死了 步子太大) /   **[ES 5](https://zhuanlan.zhihu.com/p/24336831)** 步子小  === 》**[ES](https://zhuanlan.zhihu.com/p/24570791) [6](http://es6.ruanyifeng.com/)** 步子合适 

​			（采取了 *Rails 社区[Ruby语言]* 发布的 *CoffeeScript* 部分特性） 【**ECMAScript 2015** 】 

​			---------现代编程语言---JS集大家之所长

​			之后每年更新一版本

​			核心 **ES3** --->增加 =>ES5 -->增加 =》ES6 ...

> option chain 功能简化代码判断值是否存在：
>
> > Ex ： var a= window.obj  ; if( a.b!==undefined && a.b.c!==undefined ...) {console.log(a.b.c.name)}
> >
> > option chain ： 即 console.log(a?.b?.c?.name)

---

## 二、数据类型

  有 7 中数据类型 ： 6 种基本数据类型 :number (数字64位) string(字符串16位) boolean( 布尔) 

​	[symbol](https://zhuanlan.zhihu.com/p/22652486) (符号 ES6 )  null undefined +1 种复杂类型  object (对象 包括 array 与 function)		

### number :

​	*十进制* ：  **1   1.1  .1**

​			科学计数法： **1.23e2**  、1.23E2// 1.23*10^2

​	*二进制* ： **0b** 开头  0B

​	*八进制*：   前导**0** 11 （后来 ES5 添加了 0o11 语法）// 存储电话如 010-XXX 最好用字符串保存

​	*十六进制*：**0x** 开头  0X

> 数值范围 : (2^-1023,2^1024)  ==> 正向溢出 值为 **Infinity**   负向溢出 值为 0
>
> ​	0 * Infinity   // NaN  除此其他情况正常逻辑
>
> > +0 === - 0 // true    区别在于分别当分母 结果分别为正,负无穷

> **NaN** 是 number 类型   **isNaN()** 判断 **数字类型** 是否为 NaN



### string:

​		''  // 空字符串 长度0 

​		' ' // 空格字符串 长度1

​		转义字符 \     `'\''`转义结果为 `‘` 长度为 1

​		多行字符串  \ +enter换行  //　这种写法坑人　回车中加入空格等看不见　长度会变

​							// 推荐写法　  '123' + enter '456' 

​		ES6 中 多行可以用 ``` `包裹多行字符串 其中回车也占1个长度

> 对于字符串与数组的关系  `var s='hello' ; s[0] // ‘h’`  仅仅只能查询而不是增删改
>
> length 属性同上 `s.length // 5`

### boolean:

> ​	布尔 逻辑学 下雨没带伞上课 得了肺病而逝

​	 布尔有两个值 ： true / false    真值表  &&  ||

> 布尔值常用于if 后面的判断条件，预期为布尔值 所以常自动转换

​	这6个转化为布尔值为 false   `undefined` `null` `false` `0` `NaN` `""`或 `''`[空字符串]

​		`{}` `[]` 均为 true 

### null:   空值

​	对象不赋值 推荐 赋 null  表示**空对象**   【惯例】 // var obj1 = null

​	typeof 时显示 'object‘ 【Bug】

​	typeof fn 时 显示 'function' 【Bug】

​	`undefined == null //true`

### undefined:没有值

​	1. 未赋值的变量 值为 undefined      【语法】

​	2. 对非对象不赋值 推荐赋 undefined    【惯例】

​	综合1 & 2 ==》 非对象变量不需赋值   直接 var abc;

>  条件判断中： `if(!undefined){...}`**[特例] ** 其他非单独使用不为 0  
>
> >  `5+undefined` // NaN  `if(!undefined+5)｛不执行｝`	
> >
> > `null ` 则一直为 false / 0

### 对象 :

​	分为 狭义的对象（object） 数组 （array） 函数 （function）三个子类型

​	1. 由基本数据类型构成 。对象形式====》 哈希表  k : v , k : v ... 

> var name = 'frank' ;var age = 18  ; ...  ===> 构成关系 var person = { name : 'frank' , age : 18 ...};

​	2. key 一律是字符串 它的命名: 不加引号 需要满足标识符的规范 。加引号基本没限制。

> 例如 ： var person = {'':'Bob'}  ; person['']  // 'Bob' 
>
> 特殊： 数值键名  会自动转为 字符串

​	3. 访问 value 语法 ： `obj[key]` (for in语法访问Val 用这个 而不是`.`语法)  打印输出结果顺序随机

​					`person['name']`   //注意 不加引号代表 `[ ]` 内是个存字符串的变量名

​			不加引号的 非数字值key可以：　person.name 取值V

> object['key'] 可以写作 object.key   	 注意 object.key 与 object[key] 不同

​	4. 删除key   `delete person['name']`  `person.name //undefined`  `'name' in person //false`

> 拓展 ： 循环引用 
>
> var person = {name:'frank'  }   
>
> ​	person.self = person;	 //循环引用自身 无限次...
>
> ​	person.self.self.self.name   // 'frank' 
>
> - -   var a={self : a }  //这种首先是 声明a ->1. a 为undefined ->2. a = {self:a}  //self:undefined

---

### typeof:

​	除了function 、null 检测类型为 function /object .其他都正常

​	通常利用typeof检测一个没有声明的变量 `v //报错``typeof v //不报错`

​		用在判断语句 ： `if (typeof v === "undefined"){...}`

----------------------------lesson20 end-------------------------

## 三、内存图

>  **预览**： *类型转换	各类型的API	内存图	深复制是什么鬼*

###  7种类型互相转换

#### 	**Any ---> number**:

​		1. `Number('1')`

​		2. `parseInt (‘1’,10)`

​		3. `parseFloat(‘1.23v’)` // 1.23

​		4. `‘1’ - 0`  *[老司机]*

​		5. `+‘-1’`   *[老司机 取正]*

​		

#### 	**Any ---> string** :

​		1. ` Any.toString() 方法`   [null,undefined 两种没有这个API,会报错]

​		2. `window.String(Any) 方法 `

​		3. `'` + Any    *[老司机 万能]*



#### 	**Any ---> boolean**

​		1. `Boolean(Any)`     5个 **falsy** 值：0 NaN  null undefined  **''**   [所有对象都是 true]

​		2. `!!Any`  [老司机]



### 1  VS Obj    （1 转 对象）

​		------>内存图  1 和 Obj 在计算机内存中存储方式

​	*内存分为 代码区 | 数据区  。  数据区 分为   **Stack** 栈内存| **Heap** 堆内存 。堆能存的数据很大*

​	*基本类型*  都存在 **栈**中 ，*复杂类型*  **存Heap地址**入**栈**中 (string类型堆栈都可以这种说法不算错)

```javascript
 //运行一段代码 ： 1  变量提升    2 运行 代码    代码放代码段 ,栈按一定顺序存放 堆存放随机

	代码区				   栈              堆

 var a = 1          a|   1      |                       |

 var b = 2		    b|   2      |    {...}  [100]       |

 var o = { ... }   o | ADDR 100 |                       |
```

​	*若对象存 栈 中 修改对象数据会很慢 因为要算 给多少栈内存 偏移多少个地址再存下一个数据* 	

​		object 存入 地址100  ， object 是**对象**的**引用**

#### **关于引用**：

​	**4 个内存问题**  ：【画图解决】

```javascript
var a = 1    // a 存 1
var b = a 	 // b 存 1
b = 2		 // b 存 2
a ?			 // a 是 1
```

``` javascript
var a = {name : 'a'}  // a 存 该对象的地址20
var b = a			  // b 存 a的值 地址20
b = {name : 'b'}	  // b 存 对象2的地址21
a.name ?			  // 所以a.name ='a'
```

``` javascript
var a = {name : 'a'}  // a 存 该对象的地址20
var b = a  			  // b 存 a的值 地址20
b.name = 'b'		  // 找地址20的name -》name='b'
a.name ?			  // 所以a.name ='b'
```

``` javascript
var a = {name : 'a'}  // a 存 该对象的地址20
var b = a			  // b 存 a的值 地址20
b = null			  // b 的值 清空  浏览器把b=null 记下来 不管b的值都清空掉
a ?					  // a 存的 地址20
```



**循环引用**：

``` javascript 
 var a= { }   // a 存 ｛｝的地址50
 a.self = a   // 将 地址50 给 a.self【即 地址50对象的属性 self】 => a={self:{self:{...}}}
 a.self.self  // {self:{...}} 无限循环引用下去
// 错误使用写法：↓↓↓
var a = {self : a}  // ->首先 声明变量 a 然后 地址66给 a
// a = {self:undefined}  声明的a 值为 undefined
```

**引用类型**：	

``` javascript
//例子：  等号左边 地址 计算前就确定了。 浏览器左->右看 再 右->左 计算赋值
var a = {n:1};     // a 存地址20  -->{n:1}
var b = a;		   // b 存地址20  -->{n:1}
a.x = a = {n:2};   // a 的地址均为20 -->{n:1} 然后
				//  a存地址 21-->{n:2}  地址20的a.x 存21 --{n:1,x:{n:2}}
	alert(a.x); //	undefined  a地址为21-->{n:2} 即 a.x=undefined
	alert(b.x); // [object Object]  b的地址为20-->{n:1,x:{n:2}} 即[o O]
```



#### **GC 垃圾回收**

​	**若一个对象没有被 引用 则是垃圾 等待系统回收**

``` javascript
var a = {name : 'a'}  //a 存地址20
var b = {name :'b'}	  //b 存地址30
a = b   //a 存地址30  则 地址30--》{name : 'a'} 未被引用
// 地址30 就是垃圾 等待回收

例子2：
var fn = function(){}  //fn 存地址 20
document.body.onclick = fn  //document存body地址->body->onclick ->地址20
fn = null  // fn 清空 
	// 地址20 的对象 不是辣鸡 因为还被document 引用
当把该页面关掉   则是辣鸡 因为 document 不存在了 
```

> **注意** ：
>
> **IE 6左右 BUG** : 页面关闭时 body->onclick ->function ***未标记****为垃圾【算法问题】回收* [内存泄漏]，除非关I闭整个IE
>
> 解决办法：**关闭页面前 在该方法中  要写上所有的相关事件置为null**
>
> ``` javascript
> window.onunload = function() {
>     document.body.onclick = null ...
> }
> ```



#### 深拷贝 VS 浅拷贝

``` javascript
var a = 1 
var b = a 
b =2   
a ？//a =1 深拷贝：b变 不影响 a的值 
```

 基本类型 的赋值 就是 深拷贝。所以这里研究的是 复杂类型

**浅拷贝** ：b 变 影响 a  的值

``` javascript
var a = {name : 'a'}
var b = a 
b.name = 'b'
a.name ？ // 'b'   浅拷贝:b变 影响 a的值
```

**深拷贝 ：b 变 不影响 a  的值**

​	**即对 Heap 内存进行完全的拷贝**

``` javascript
var a = {name: 'a'}
var b = deepClone(a) // deepClone 还不知道怎么实现
b.name = 'b'
a.name === 'a' // true
```



----------------------------lesson21 end-------------------------

​	

## 四、js 里的对象

> 预览：全局对象 window    全局函数   公用的属性藏在哪     toString 是哪来的 -------原型链



#### 一、全局对象window

​	全局对象标准规定：  **global** (浏览器中全局对象：window)

``` javascript
浏览器 生成 global 栈存入地址20  地址20指向堆内存中window对象
```

**window 的属性** 分为 

1. **公共[ECMA规定必有的]**   

> 如 parseInt ... Number() String() Boolean() Object() ...

2. **私有的(各个浏览器自己的API等)两类**

> alert prompt confirm console  history[*BOM*]
>
> document(文档) --**-DOM规范** 由*W3C*制定

```javascript
var n = Number()  
n? // 0
var s = String()
s? // ''
var b = Boolean()
b? // false
var o = Object()
o? // ｛...｝
 -----
  //异步函数 计时器: 会有自己的编号
    window.setTimeout(function(){console.log(1)},3000)
	window.history.back()  //浏览器页面会后退操作
```



#### 二、简单类型变为对象

​	**结论**：JS 中直接使用 **变量 n** ，不需要包装为简单类型对象。因为js会自动利用**临时对象**来处理。

1. 简单类型**转对象** 与**简单类型**本身有什么**区别**？   :包装为对象有便捷的API可以使用

    	**1**  VS  **new  Number(1)**

   **Number() 的用法** ： ①：转为数字 Number('1')  // 1

   ​				    ② : 声明一个number 对象

   ``` javascript
   var n1 = 1   // n1 存 1
   var n2 = new Number(1) // n2 存 地址20 =》{valueOf():1 ...}
   
   // n1 n2 区别就是 n2 是个数值型对象，有便捷的API可以使用
   n2.toString() //'1'
   n2.valueOf()  // 1
   n2.toFixed(2) // '1.00'
   n2.toExponential() // '1e+0'
   ```

   **历史回溯：**

   ``` javascript
   //boss : js 长的像 Java ===》var n = new Number(1) 这种没人用
   
   //BE : var n =1 ==> JS  但是缺点: n.toString() 不行
   	妙计： 利用临时转换：
    原理：  var temp = new Number(n)
          	temp.toString() //运行结果给n.toString() 后抹杀temp
   
   ===》 var n = 1
   		n.xxx = 2  //2 执行时创建temp返回 执行完一条语句就抹杀掉temp
   		n.xxx ? // undefined  所以这里再次运行创建temp temp没有xxx属性
   ```

   **string boolean 同理**

   

   **string** :

   ``` javascript
   var s = 'hello'
   s[0] //'h'  key 按顺序0-x的 hash似
   s.anchor() //<a name='undefined'>hello</a>
   s.big() //<big>hello</big>
   
   s.charAt(1) //'e' 第2个字符
   s.charCodeAt(0)  //104
   'a'.charCodeAt(0) //97  十进制的 a
   'a'.charCodeAt(0).toString(16) //'61' 16进制的 a
   (100).toString(16) //'64'  将100转为16进制的字符串 6*16+4*16^0
   ```

   ``` javascript
   // 字符串的一些方法
   ' user  '.trim()  //1、去掉首尾多余的空格
   
   var s1 = 'hello'
   var s2 = 'world'
   //2、链接 s1 + s2
    s1.concat(s2) //'helloworld'
   	s1? s2? //'hello' 'world'  
   //3、切片 取字符 [0,5) -->包前不包后
    s1.slice(0,5) //'hello'
    s1.slice(0,6) //'hello'
   //4、 替换字符
    s1.replace('e','o') //'hollo'
     s1  ? //'hello'
   ```



​	**boolean**:

```	java
var b = new Boolean(true)
    b.toString() //'true'
    b.valueOf() //true
    
//   踩坑： 
   var f = false 
   var f2 = new Boolean(false)
   if(f){console.log(1)}
   if(f2){console.log(2)}
 	// 结果： 2   f2是｛｝所以f2会打印
```

​	**复杂类型：**

```	javascript
var o1 = {}          //o1 ->{}
var o2 = new Object() // o2 -->{}
o1 === o2 //false 
	它们俩完全没区别，但是不相等。因为存的地址不一样 引用的堆位置不一样
```



#### 三、公用的属性藏在哪

​	由**重复的属性** 占据内存 --》**复用** 重复的属性*[将其打包放入名为共有属性/原型的对象中]*

其他对象直接*[存 包含共有属性的对象 地址即可]***引用**   避免重复属性导致内存浪费

``` javascript
var n = new Number(1)   //addr 10
var s = new String('hi')  //addr 20
var b = new Boolean(true)  //addr 30
var p = new Object()	 //addr 40
//创建这些对象时，它们都含有 toString,ValueOf 属性。大量重复占据Heap空间

//js不这样做：将这些共有属性集合为 x, 让n/s/b/p相应的key 指向 x 的地址
//js 这样做：在创建对象时，创建了隐藏属性__proto__,其指向 包含共有的属性对象
  //js 两步走： 1. hash 声明好  2. 绑定下各自的共用属性（原型）
```

**toString 是哪来的 -------原型链**

``` javascript
var o1 = {}
var o2 = new object()
o1 === o2 /false
o1.toString === o2.toString  //true  o1 o2 的 tostring 是同一个
```

​	 **o1.toString() 执行轨迹：**

``` javascript
o1.toString() //o1 若不是对象 则转为临时对象 
//o1 若是对象，去找toString().找不到 则去__proto__找。
//__proto__它引用共用属性，在共用属性里面去 层层往上一层，一直找到根节点Object 若还是没有则确定为没有该方法。
```

**分歧**：当一个**不普通的对象**[如简单类型包装]：

``` java
var n = new Number() //n的__proto__ 中包含 toFix/toEx../toString(16)【能接收参数】
var o = new Object() //n数字型对象 的共有属性与 o 普通对象的并不一致
  // string boolean 等类似 它们都有各自的共有属性    
```

​	普通对象没有自己特有的API 



​	**简单型对象的方法执行轨迹：**

``` javascript
例如：
var n = new Number() //n.__proto__指向 Number.prototype[①]
//Number.prototype.__proto__指向 Object.prototype[②] ①②这就是原型链
//Object.prototype.__proto__指向 null

var o1={}  //普通对象
o1.__proto__ === Object.prototype //true

var n1 = new Number(1) // 数字型对象  字符布尔型对象同理
o1.__proto__ === Number.prototype   //true
o1.__proto__.__proto__ === Object.prototype  //true
```



#### 四、原型与GC

 由于 生成对象 绑定*[Stack]* **-->** 共有属性*[Heap]*保证了**引用关系**。

​	当**无代码**时 内存中 共有属性若`无引用关系` 则会被**GC**

​	无代码内存情况：`window [Stack] `  `Number/Object...[Heap]`

​	window 存 包含Number等属性集合的地址 **Number.prototype** 指向的 是 number 的共用属性.

它只是指向。保持不会被gc .保证了无代码状态下共有属性的存在。

``` javascript
var s = new String(1)
String.prototype 是 String 的公用属性的引用 //String.prototype 指向的 是 String 的共用属性.
s.__proto__ 是 String 的公用属性的引用 // s.__proto__ 指向的 是 String 的共用属性.
//区别：第一个 防止 原型GC  第二个是要用的
```

---------------------

*烧脑环节：*

#### 重点公式：对象.__proto__ === 函数.prototype

``` javascript
首先理解：
var __1__ = new __2__  //1 类型应为 对象 'object'  2 类型:函数对象

然后就知道 __p__ 和 .prototype

var 对象 = new 函数()
公式： 对象.__proto__ === 函数.prototype  //它们俩指向同一个对象
//__p 和 p 区别: 就是 _p 是对象的属性, p 是函数的属性

typeof Function //'function'
Function.__proto__ === Function.prototype  //true
Function.__proto__ === Object.prototype   //false

由公式可推导：
函数.__proto__ === Function.prototype 
Function.__proto__ === Function.prototype //Function的_p和p 指向同一个地址
Function.prototype.__proto__=== Object.prototype //Function共有属性的_p指向Obj的p


Object.__proto__ === Function.prototype，因为 Function 是 Object 的构造函数。

//总结： Function = { p:a30, _p:a30}  => a30:Function.p
	//a30:{ _p:a40，...} => a40:obj.p  a40:{ _p =null}
	// obj{p:a40,_p:a30}
```

-------------------------------lesson22 end

