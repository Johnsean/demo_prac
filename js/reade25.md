# Lesson 25.  JS标准库---- 数组



## 一、 new & 无 new 



###  1. 标准库

> JS 内存 1.Stack 2.Heap .   
>
> Stack 存 全局变量global /window 。全局变量指向堆内 一个哈希表。
>
> 哈希表内 ： 标准库 & 非标准库 
>
> ​	    标准库：*String() Number() Boolean() **Array()  Function()***   *Object() [内有keys() create()...]*

---



###  2. 有无new 区别

`简单类型 `& `Object` 区别：即 *简单类型* & *Array/Function/Object* **区别** ：

​        在于： new 关键字. 👇👇👇

>  对象() ===> 对象 |    new +对象() ===> 对象  
>
> 简单类型() ===> 简单类型  | new +简单类型() ===> 对象
>
> Ex：

``` javascript
 // 简单类型
String(1)  // '1'  ==>string
String(true) //'true' ==>string
String(null) //'null' ==>string
new String(1) //String{[[PrimitiveValue]]: "1"} ==> Object string
new Number(123)//Number{[[PrimitiveValue]]: 123} ===>object number
 
 // 对象
Object(1) //Number{[[PrimitiveValue]]: 1}  ==> Object number
Object('sss') // String{[[PrimitiveValue]]: 'sss'} ===>object string
Object() // {}  ===>object
new Object() // {}  ===>object
```



## 二、 Array 基本用法 

​	**window.Array** 全局对象（也是函数）

**语法**：`[element0, element1, ..., elementN]`

​        `new Array(element0, element1[, ...[, elementN]])`

​        `new Array(arrayLength)` 	*arrayLength 一个范围在 0 到 2^32  -1 之间的整数*

``` javascript
 // 有无new 一样的效果
let f = ['a', 'b'] // ['a', 'b']
let f2 = new Array('a','b') // ['a', 'b']

Array(3) // {length:3} 
Array(3,3) // [3,3] 
new Array(3) 跟不加 new 一样的效果
new Array(3,3,) 跟不加 new 一样的效果
```

**属性**: `length` 属性不一定表示数组中定义值的个数。

``` javascript
var a = []
a.length = 10 //[empty*10] 
// 可通过length 来扩大/截断数组数据 

// 下标大于设置后的length 的数据 将被截断
a=[1,2,3,4,5,6]
a.length = 2; //[1,2]
```





### 1. Array()不一致性 [辣鸡]

`Array(3)`& `Array(3,3)` 

``` javascript
//一个参数：生成length为 arg1的空数组。无key {length:arg1,__pro__}

//多个参数：生成key从0开始的数组 {0:arg1,1:arg2...length:argN-1,__pro__}

//----- 第一个参数是length
var a = Array(3) //[empty*3] ==> length: 3  __proto__:Array(0)
	a[0]? a[1]? '0' in a ? // undefined undefined false
a.__proto__ === Array.prototype // true    

//----- 第一个参数不是length
var a = Array(3,3) //[3.3]
```



### 2. Function() 

​	window.Function 全局对象（也是函数），lastArg 为函数体.

``` javascript
var f = Function('x','y','return x+y')
f(1,2) // 3
var f = new Function('x','y','return x+y') // 跟不加 new 一样的效果
f(3,4) // 7
```

#### **函数声明方法总结**:

1. 具名函数  ` function f(x,y){ return x + y }`

2. 匿名函数  `function(x,y){ return x+y }`
3. window.Function  `new Function('x','y','return x+y')`

function 关键字[if else var function]  声明一个函数

Function 全局对象



---

## 三 、 JS 中数组

### **1 . JS 中数组的本质**

> 人类理解：数组就是数据的有序集合
>
> JS理解：数据就是原型链中有 **Array.prototype** 的对象

``` javascript
a=[1,2] //数组a |0:1 1:2 length:2 __p| __p=>Array.prototype
   //Array.prototype |push pop __p...| __p=>Object.prototype

obj={0:1,1:2,length:2}
//对象obj |0:1 1:2 length:2 __p| __p=>Object.prototype

     对象.__p === 函数.prototype
```

### **2. 伪数组**

1. 有 *0,1,2,3,4,5...n,length* 这些 **key** 的对象

2. 原型链中没有 **Array.prototype** 这样的对象就是伪数组

> 目前知道的伪数组有 ： 1.    *arguments* 对象  
>
> 2. *document.querySelectAll('div')* 返回的对象



``` javascript
Object.prototype | toString valueOf
--String.prototype | .trim() .split() ...
--Function.prototype |.call() .bind() .apply()
--Array.prototype | .push() .pop() .shift() .join() ...
```



### **3. 数组的** **API**

**不改变原数组**：***join(char)  slice(start,end) concat(array)*** 



**Tips**:  对于某些迭代API：不要尝试在遍历过程中对原数组进行任何修改

*一些迭代方法* *【都需要指定一个回调函数作为参数。在每一个数组元素都分别执行完回调函数之前，数组的length属性会被缓存在某个地方】*

>1. Array.prototype.forEach  //遍历并将每个元素及索引传递给一个函数
>
>2. Array.prototype.sort   //按字母表顺序排序 可传排序方法
>
>3. Array.prototype.join  // 将元素拼接为一个字符串 原数组不变
>
>4. Array.prototype.concat //拼接数组 原数组不变
>
>5. Array.prototype.toString  //返回一个字符串，逗号隔开
>
>6. Array.prototype.map //回调函数返回值组成一个新数组返回 原数组不变
>
>7. Array.prototype.filter //回调函数返回值为真的 组成一个新数组返回 原数组不变
>
>8. Array.prototype.reduce //前两项作为参数计算结果作为第一个参数 迭代循环
>
>9. [见 MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)

> > reverse()  //逆序
> >
> > slice [start,end) // 返回数组中一个片段。arg值为负数则倒数, start>end则返回[ ] 

> > > ***ES5:***
> > >
> > > 1. *Array.**isArray(obj)***  //判断一个对象是不是数组 **true/false**
> > >
> > > 2. *.indexOf()*  // 查找到第一个后返回其索引，没有查找到返回 **-1**
> > >
> > > 3. *.lastIndexOf()* // 反向搜索
> > >
> > > 4. *.forEach(element, index, array)*  //遍历，参数为回调函数，回调函数有3个参数
> > >
> > > ``` javascript
> > > var a = new Array(1,2,3,4,5,6)
> > > a.forEach(function(e,i,array){ array[i]= e + 1;});
> > > 
> > > a //[2, 3, 4, 5, 6, 7]   改变原数组
> > > ```
> > >
> > > 5. 形式同forEach的 *some & every*
> > >
> > >    ``` javascript
> > >    //every 每个回调函数都返回true的时候才会返回true
> > >    //some “存在”一个回调函数返回true的时候终止执行并返回true
> > >    在空数组上调用every返回true，some返回false
> > >    ```
> > >
> > > 6. *map(function(element))* //同forEach 但回调函数返回值组成一个新数组返回
> > >
> > >    ``` javascript
> > >    //原数组不变，新数组索引结构和原数组一致
> > >    var a = [1, 2, 3, 4, 5, 6]
> > >    a.map(function(e){return e * e})  // [1, 4, 9, 16, 25, 36]
> > >    
> > >    a //[1, 2, 3, 4, 5, 6]
> > >    ```
> > >
> > > 7.  *filter(function(element))* // 但回调函数返回值j结果为true的组成一个新数组返回
> > >
> > >    ``` javascript
> > >    //新数组只包含返回true的值，索引缺失的不包括，原数组保持不变
> > >    var a = [1, 2, 3, 4, 5, 6]
> > >    a.filter(function(e){ return e % 2 == 0;})// [2, 4, 6]
> > >    
> > >    a //[1, 2, 3, 4, 5, 6]
> > >    ```
> > >
> > > 8. *.reduce(function(v1, v2), value)* // 按索引值最小的开始，v1=f(v0,v1) 迭代v1=》结果
> > >
> > > ``` javascript
> > > //[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)
> > > //1.回调函数：把两个值合为一个，返回结果
> > > //2.value，一个初始值,可选
> > > var a = [1, 2, 3, 4, 5, 6]
> > > a.reduce(function(x, y){ return x + y })  // 21
> > > 
> > > var arr = [1, 3, 5, 7, 9];
> > > arr.reduce(function (x, y) {return x * 10 + y;}); // 13579
> > > ```
> > >
> > > 9. *.reduceRight(function(v1, v2), value)* //索引值最大的开始
> > >
> > >    ``` javascript
> > >    var a = [1, 2, 3, 4, 5, 6]
> > >    a.reduceRight(function(v1, v2){ return v1 - v2 }, 100) //79
> > >    ```



---

`splice `  一次性解决数组添加、删除（这两种方法一结合就可以达到替换效果）

>1.开始索引    2.删除元素的位移[执行多少次删除操作]
>
>3.插入的新元素，当然也可以写多个

返回一个由删除元素组成的新数组，没有删除则返回空数组

```javascript
var a = new Array();
a[2]=2; a[3]=3; a[7]=4; a[8]=5;
a.splice(3,5); //[3,empty*3,4]  从Index3开始执行5次删除操作
a //[empty*2,2,5]
```

