# Lessons.20-21

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

	> 运行一段代码 ： 1  变量提升      2 运行 代码    代码放代码段 ,栈按一定顺序存放 堆存放随机
	>
	> 代码区				   栈                   堆
	>
	> var a = 1                    a|   1             |                             |
	>
	> var b = 2		    b|   2             |    {...}  [100]       |
	>
	> var o = { ... }	     o |ADDR 100 |                            |

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

----------------------------lesson21 end-------------------------

​	

​	











## 四、对象