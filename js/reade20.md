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

  有 7 中数据类型 ： 6 种基本数据类型 :number (数字) string(字符串) boolean( 布尔) 

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

> 拓展 ： 对象里面放自己 然后调用 
>
> var person = {name:'frank',... , self : person }  
>
> person.self.self.self.name   // 'frank'

---

### typeof:

​	除了function 、null 检测类型为 function /object .其他都正常

​	通常利用typeof检测一个没有声明的变量 `v //报错``typeof v //不报错`

​		用在判断语句 ： `if (typeof v === "undefined"){...}`



## 三、内存图

## 四、对象