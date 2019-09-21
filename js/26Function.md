# Lesson 26. Function 函数



*函数是可以执行代码的对象*

**函数默认返回值为undefined ===> {return undefined}**

>console.log(x)   x 不为字符串时，调用x的 toString方法
>
>函数原型的 *toString* 方法是将本身字符串化

## 一、 函数五种声明方式&name属性

1.  具名函数

``` javascript
 function f(x,y){return x+y}
     f.name // 'f'
```

2. 匿名函数

``` javascript
var f2
f2 = function(x,y){return x+y}
	f2.name // 'f2'
```

3. 具名函数赋值

``` javascript
var f3
f3 = function f4(x,y){ return x+y }
	f3.name // 'f4'
	console.log(f4) // 报错f4没定义 -->f4的作用域只在该函数体内
	console.log(f) //同1 对比 不报错，f作用域全局----js不一致性 辣鸡
```

4. window.Function

``` javascript
var f5 = new Function('x','y','return x+y')
	f5.name // "anonymous"
```

5. 箭头函数

``` javascript
var f = (x,y) => {return x+y}  //完全式
var sum = (x,y) => x+y  //俩去式 单语句
var n2 = n => n*n //单参数 可无括号
```



## 二、如何调用

函数原型的属性有: `call bind apply toString`

>1 .`new Function(params,fbody)`  堆内函数 fn 存参数，及函数体字符串形式。
>
>2 . `fn.__p__`指向函数原型 调用其 `call()`方法即是执行函数体。
>
>3 .`eval()`将字符串当做代码执行 ==》` fn.call =function(){return window.eval(f.fbody)}`
>
>4 . 调用时 执行：` fn.call()` 即将函数体字符串当做代码执行  |*调用函数的过程==》eval函数体的过程*

​      

**fn ==>对象  fn() ==>fn.call() 的糖 //执行函数体**



## 三、this arguments



`fn.call(undefined,1,2)`   // *this*   [1,2] *arguments*

> *call* 第一个参数 可以用*this*得到  
>
> *call* 后面的参数可以用*arguments*得到  

``` javascript
f.call(asThis, input1,input2)
//其中 asThis 会被当做 this，
//[input1,input2] 会被当做 arguments
//禁止使用 f(input1, input2)
```

``` javascript
//严格模式下 第一个参数用this 得到 
f = function() {
    'use strict'
    console.log(this)
    console.log(this===window)
}
f.call(undefined) // unedfined ; false
f.call(1) f.call('sss') //1 ;sss ;false;false
//普通模式下 若第一个参数为undefined 则浏览器把this 变为 window
将上面'use strict'去掉
f.call(undefined)  // window ; true
```



***arguments*** 是伪数组。

> 它的 *_p* 指向的是 *Object*原型。|| 没有指向数组原型。|| 没有*push()*方法 。

*伪数组即整个原型链中没有Array.prototype即为伪数组*



## 四、call stack

**压栈： 先进后出**  *或者说 后进先出*

栈溢出：*-stack overflow*   压入的超过栈内存大小

> [普通调用](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gYSgpewogICAgY29uc29sZS5sb2coJ2EnKQogIHJldHVybiAnYScgIAp9CgpmdW5jdGlvbiBiKCl7CiAgICBjb25zb2xlLmxvZygnYicpCiAgICByZXR1cm4gJ2InCn0KCmZ1bmN0aW9uIGMoKXsKICAgIGNvbnNvbGUubG9nKCdjJykKICAgIHJldHVybiAnYycKfQoKYS5jYWxsKCkKYi5jYWxsKCkKYy5jYWxsKCk%3D!!!) [顺序压栈]、
>
> [嵌套调用](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gYSgpewogICAgY29uc29sZS5sb2coJ2ExJykKICAgIGIuY2FsbCgpCiAgICBjb25zb2xlLmxvZygnYTInKQogIHJldHVybiAnYScgIAp9CmZ1bmN0aW9uIGIoKXsKICAgIGNvbnNvbGUubG9nKCdiMScpCiAgICBjLmNhbGwoKQogICAgY29uc29sZS5sb2coJ2IyJykKICAgIHJldHVybiAnYicKfQpmdW5jdGlvbiBjKCl7CiAgICBjb25zb2xlLmxvZygnYycpCiAgICByZXR1cm4gJ2MnCn0KYS5jYWxsKCkKY29uc29sZS5sb2coJ2VuZCcp!!!) [执行a 执行 b  执行c  结束 回到b  结束 回到 a ]、
>
> [递归调用](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gc3VtKG4pewogICAgaWYobj09MSl7CiAgICAgICAgcmV0dXJuIDEKICAgIH1lbHNlewogICAgICAgIHJldHVybiBuICsgc3VtLmNhbGwodW5kZWZpbmVkLCBuLTEpCiAgICB9Cn0KCnN1bS5jYWxsKHVuZGVmaW5lZCw1KQ%3D%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D) [循环顺序执行调用反复]。



一句语句执行完则该句出栈，return 则函数出栈



## 五、作用域

### 1. 数据结构 Tree

*数据结构优先级 > 算法*

*数据结构好不好 决定 对代码抽象程度够不够*

> 哈希表 队列 链表 堆（子集树） 栈    



### 2. 就近原则 & scope & 变量提升 & 时效性

找变量，按照语法树，**就近原则**

1 .作用域是一个 ***tree***   2 .每一个 *function*  即一颗 ***tree*** ， ***tree*** 内的 *function*为其子树

> > 1. 函数在执行的过程中，先从自己内部找变量 [自身scope内声明的]
> > 2. 如果找不到，再从**创建当前函数所在的作用域**去找, 以此往上
> > 3. 注意找的是变量的当前的状态  // 变量的值随着时间[时间影响赋值]会变化

3 . 每颗树内 `var 变量 = 5` 将变量声明提前；未声明的 在其父树内找，找不到则层层向上。

​                                                                              若 其根树也没有,则声明并赋值

```javascript
//面试题1
var a = 1           //全局: var a; a = 1
function f1(){        //f1: var a ;
    alert(a) // 是多少  // undefined
    var a = 2        // a = 2
}
f1.call()
//拿到代码直接做——必然会错。请先提升声明
```

``` javascript
//面试题2
var a = 1           //全局: var a; a=1
function f1(){     //f1: var a ; a=2
    var a = 2
    f2.call()
}
function f2(){     //a 为全局a =》 1
    console.log(a) // 是多少
}
f1.call()
```

``` javascript
//面试题3
var liTags = document.querySelectorAll('li')
for(var i = 0; i<liTags.length; i++){
    liTags[i].onclick = function(){
        console.log(i) // 点击第3个 li 时，打印 2 还是打印 6？
    }
}
//绑定完监听后i 为 6  所以点击第3个 li的时候 i为6
console.log(i) //6
```



***只能确定变量是哪个变量，但是不能确定变量的值，因为哪个变量由其scope决定，其值由执行时[时间有效性]确定。***

``` javascript
// 时效性 由执行时确定值。而变量是哪个范围域[哪颗树内的]的变量则一直都是确定
EX1 :
var i = 2 
setTimeout(function(){console.log(i)},10000)
?？？？？  //其他代码
// 定时器执行结果由十秒后 代码i的值决定。10秒内 i可能执行了其他赋值语句
```



## 六、闭包

如果一个函数，使用了它范围外的变量，那么（这个函数+这个变量） 就叫做[闭包](https://zhuanlan.zhihu.com/p/22486908)

``` javascript
var a = 1
function f4(){
    console.log(a)
}
// 闭包
```

