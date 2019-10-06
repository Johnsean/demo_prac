# Lesson 35 实现AJAX

1.  js 设置 请求 获取 响应 
2. 封装ajax  & 优化 `将原生AJAX 封装入函数 & 库，传参。`  `参数命名为对象，传入结构化的对象`
3. Promise 用法  `return new Promise(function(resolve,reject){})`



## 一、用AJXA 获取：获取 请求 & 响应 的各部分 1 2 4 部分：

·         客户端的 **JS**发起请求（浏览器上的）

·         服务端的 **JS** 发送响应（Node.js上的）

### 请求：

``` javascript
Request Headers
GET /xxx HTTP/1.1

HOST: jack.com:8002

Content-Type: application/x-www-form-urlencoded
...
```



> ***Chrome*** 默认 **GET** 请求没有[不显示]body ，想看到设置 'post' 打印
>
> ``` javascript
> Request Payload   //请求第四部分
> 	body // send(body) 的内容
> ```

``` javascript
// 第一部分   //与上面对应 GET /xxx HTTP   HOST
request.open('get','http://baidu.com/xxx')     

//第二部分 设置HTTP请求头。 设置多个 会合并为1个发出

request.setRequestHeader('frank','18')
request.setRequestHeader('Content-Type','x-www-from-urlencoded')

request.send()  ... // 第四部分 .send(body) 

```



### 响应：

``` javascript
Response Headers
HTTP/1.1 200 OK

Content-Type: text/json;charset=utf-8
Access-Control-Allow-Origin: http://frank.com:8001
...
```

``` javascript
//第一部分 
request.status // 200
request.statusText  //OK
//第二部分 
request.getResponseHeader('Content-Type') // 获取Content-Type响应头的值
request.getAllResponseHeaders() //获取所有响应头
//第四部分 
request.responseText  // string
```



## 二、封装AJAX

1. ` window.jQuery = function(){...}`
2. `window.jQuery = function ( url,method,body,successFn,failFn ) { 原生AJAX代码 }`

> 封装的｛｝ 内：
>
> ``` javascript
> let request = new XMLHttpRequest()
> request.open(method,url) //配置request
> request.onreadystatechange = ()=>{
>     if(request.readyState ===4 ){
>         if(request.status >=200 && request.status <300){
>             successFn.call(undefined,request.responseText)
>         }else if(request.status >=400){
>             failFn.call(undefined,request)
>         }
>     }
> }
> 
> // 调用
> window.jQuery.ajax(
> 	'/xxx',
>     'post',
>     'a=1&b=2',
>     (responseText)=>{...},
>     (request)=>{...}
> )
> ```

避免传参的------混乱 

传参设置为 一个**有结构**的数据类型 ----对象  `let obj={url:'/xxx',method:'get'...}`



### **参数命名 一般为 options：**

``` javascript
window.jQuery.ajax = function(options){
    let url = options.url
    let method = options.method 
    let body = options.body
    let successFn = options.successFn 
    ...
}
    
```

``` javascript
ES6 写法 
let {method,body,successFn,failFn,headers} = options   //ES6 析构赋值

//调用用上  省略声明options  👇相当于用let 声明 ES6语法
window.jQuery.ajax = function({method,body,successFn,failFn,headers}){}
```



3. **callback** 就是满足某种条件的**函数**

```javascript
window.jQuery.ajax(
	url:'/xxx',
    method:'get',
    successFn:(x)=>{console.log(x)}, //这种是 callback : 反方向 CALL  【反向传参】
    failFn:()=>{...}
)

// successFn.call(undefined,request.responseText) 反向它←
```

---

### jQ.ajax 接受两种参数

$.ajax(url[,settings])

$.ajax(url)  

等价于 

``` javascript
window.jQuery.ajax = function(options){
    let url 
    if(arguments.length ===1){
        url= options.url
    }else if(arguments.length ===2){
        url= arguments[0]
        options = arguments[1]
    }
 
    let method = options.method 
    let body = options.body
    let successFn = options.successFn 
    ...
}
```



### ES6 

### 值互换`  [a,b] = [b,a]`

``` javascript
f = function(a){...}

f2 = (a)=>{...}

var x = '???'
var o = {}
o[x] = true // '???' as key
           
var x = '???'
var o = { [x]:true }
```



## 三、Promise

\- **库 不看文档不知道传的参数 & 位置===**

**确定函数形式的 规范：Promise  .**  

> 好处匿名了 函数名 成功调用x  失败调用x：

``` javascript
function xxx(){
    return new Promise(f1,f2)=>{
        doSomething()
        setTimeout(()=>{
            //成功就调用f1 失败就调用f2
        },3000)
    }
}
```

JQ   $.ajax(..).then(f1,f2)   成功调用f1  失败调用f2  可链式。

`f1(responseText){ //responseText它现在是对象了 }`

且 jq封装了 若响应 返回的是json， 自动转 对象。

---

### 自己封装 then

将API body 封装到 返回值 为promise 构造的对象中------将success / fail  替换为 resolve reject。

``` javascript
window.jQuery.ajax =function({url,method,body,headers}){
    // successFn failFn 不用传了 ，改为resolve reject
    return new Promise(function(resolve,reject){
        let request = new XMLHttpRequest()
        request.open(method,url)
     	for(let key in headers){     
         	let value = headers[key]
         	request.setRequestHeader(key,value)
        }
        ...原生ajax
        // 成功  resolve.call(undefined,request.responseText)
        //失败  reject.call(undefined,request)
        
    })
    request.send(body)
} 

// headers: {'1'：‘1’,'2'：‘2’} 👆对应 遍历
```

``` javascript
调用 ：

 .then(()=>{},()=>{}).then(...)...  //可链式
```



### **window.Promise源代码：**

``` javascript
window.Promise = function(fn){
    //...
    return {
        then:function(){}
        ...
    }
}
```

