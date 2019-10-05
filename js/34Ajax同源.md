# Lesson 33 ajax是什么鬼

## 预览：

[AJAX](http://javascript.ruanyifeng.com/bom/ajax.html)

>1. 怎么发请求  ： ***form*** 表单  标签 ***a/link/script/img***
>2. **IE5** 的时候退出ActiveX对象 可以发出 http 请求 [代表：Gmail邮件] **局部更新**： 前端起源=》W3C 纳入规范 取名***XMLHttpRequest***
>3. ***json*** 是一门新语言。多出抄袭js。类型没有***undefined,function***。字符串格式**必双引号**开头结尾。
>4. ***AJAX*** 三步：a. 使用 ***XMLHttpRequest***发请求 b. 服务器返回字符串 c.解析读取 并更新页面
>5. **面试题**：原生JS发AJAX请求  :1 创建对象 2 a.***open()*** 3. a.***send()*** 4. a.***onreadystatechange(...)***
>
>``` javascript
>let request = new XMLHttpRequest()
>request.open('GET','/xxx')
>reuqest.send()
>request.onreadystatechange=()=>{
>    if(request.readyState === 4){ //响应完毕
>        if(request.status >=200 && request.status < 300){ //请求成功时
>           let string = request.responseText
>           let object = window.JSON.parse(string) //转js对象
>        }
>    }
>}
>```
>
>5. **CORS**   只有 协议+端口+域名 一模一样才允许发 AJAX 请求
>
>>只有ajax请求才有该安全策略 。 表单 标签等请求不在此列 可以跨域
>
>突破同源策略 === 跨域   ***Cross-Origin Resource Sharing***
>
>>跨域方式  ***1 . JSONP***   2. 后台 多添加一个【含有跨域标识】&&【发出请求的域名】响应头 ：
>>
>>``` javascript
>>...
>>response.setHeader('Content-Type','text/json;charset=utf-8') 
>>
>>//告诉浏览器 这是网站 是我朋友 别阻止它
>>response.setHeader('Access-Control-Allow-Origin','http://frank.com:8001') 
>>...
>>```

---

### 金句：

1. 你才返回对象，你全家都返回对象

2. JS 是一门语言，JSON 是另一门语言，JSON 这门语言抄袭了 JS这门语言

3. AJAX 就是用 JS 发请求

4. 响应的第四部分是字符串，可以用 JSON 语法表示一个对象，也可以用 JSON 语法表示一个数组，还可以用 XML 语法，还可以用 HTML 语法，还可以用 CSS 语法，还可以用 JS 语法，还可以用我自创的语法



## 一、 如何发请求？

用 form 可以发请求，但是会刷新页面或新开页面

> **Request Headers** : `GET /xxx ?password=111 HTTP/1.1`
>
> ​				 `POST /xxx HTTP/1.1`   数据在 **Form data** 中 : **password: 123**

用 **a** 可以发 **get** 请求，但是也会刷新页面或新开页面

> **缺点：**点击后要么当前页面刷新  要么新开页面刷新
>
> 1. 点击a标签 get  2. 主动帮用户点 触发click事件

用 ***img*** 可以发 **get** 请求，但是只能以图片的形式展示

> ``` javascript
> var image = document.createElement('img')
> img.src = '/xxx'
> img.onload=function(){...} //...onerror...
> ```

用 ***link*** 可以发 **get** 请求，但是只能以 CSS、favicon 的形式展示

用 ***script*** 可以发 **get** 请求，但是只能以脚本的形式运行

> -***link/script***　必须放页面里面才会去请求   
>
> ``` javascript
> var link = document.createElement('link')
> link.rel = 'stylesheet'
> link.href = '/xxx'
> // 页面中 如 head body 
> document.head.appendChild(link)
> ```



**有没有什么方式可以实现**

1. **get、post、put、delete** 请求都行

2. 想以什么形式展示就以什么形式展示

   

使用 **XMLHttpRequest** 发出http请求  / window 拥有的构造器



## 二、AJAX	

***Jesse James Garrett*** 讲如下技术取名叫做 AJAX：**异步的 JavaScript 和 XML**

1. 使用 XMLHttpRequest 发请求

2. 服务器返回 XML 格式的字符串

3. JS 解析 XML，并更新局部页面

*Async js and xml*

> **HTTP 协议第四部分 始终是字符串**
>
> **后台路由 HTTP路径path 为 绝对路径**
>
> ``` javascript
> if(path === '/'){  //绝对路径  如 '/main.js' 而不是 './main.js'
>     let string = fs.readFileSync('./index.html','utf8')
>     response.statusCode = 200
>     response.setHeader('Content-Type','text/html;charset=utf-8') 
>     response.write(string)
>     response.end()
> }
> ```
>
> 点击按钮发出 AJAX请求：
>
> **Request Headers** : `GET /main.js HTTP/1.1` //  /main.js 绝对路径



``` javascript
1.  创建对象 
let request = new XMLHttpRequest() 
request.open('GET','/xxx') // 配置request  第一个参数为请求方式 可：GET POST PUT DELETE等
reuqest.send()  //发出请求
```

``` javascript
2.  返回字符串：
request.responseText
```

``` javascript
3.  读取js字符串   //响应返回 分段/次分时 
request.readyState  : 0 open()方法未调用  1 send()未调用 2 send()调用 响应头&响应状态已返回

					3 响应体下载中 并已获取部分  4 整个请求过程完毕
  
 //我们只需要关注readState =4 ，运行代码很快 1ms检测不到每个状态变化 往往 23步骤在1ms就完成了 ======== 这个事件代码写的位置 要尽量往上放 否则会错过一些状态                 
request.onreadystatechange=()=>{
    if(request.readyState === 4){ //响应完毕
        if(request.status >=200 && request.status < 300){ //请求成功时
            // 状态码：2 开头成功  4 开头失败 3 开头会发出新的请求 具体情况具体分析
           console.log(request.responseText) // 响应第四部分字符串
        }else if(request>=400) {
            //请求失败
        }
    }    
   request.open()
    ...
```

``` javascript
4. 	解析json

let string = request.responseText // 符合JSON 语法的字符串 
let object = window.JSON.parse(string) //转换成 js对应值 

typeof request.responseText   // string
typeof object //object   
```



### console.time() ; 代码; console.timeEnd()  显示代码执行用时



## 三、 JSON一门新语言

**JavaScript Object Notation** 道格拉斯 克罗克福特  轻量级的数据交换语言  [火车图]

www.json.org

**value :** ***string number object array true false null***

**js json区别：** 

> **核心:  这是两门语言**

1. **JSON** 没有抄袭***undefined,function***。
2. **JSON ** 的字符串首尾  **必双引号**
3. JSON **没有变量**  没有原型链 `{__proto__}`  [因为json KEY是字符串必双引号]

``` javascript
后台： 这是符合json 语法的字符串  不是对象
response.write(`
{
"note":{
	"to":"小谷",
	"from":"frank",...
	}
}
`)

前端： typeof request.responseText   // string

```



## 四、同源策略

只有 **协议+端口+域名** 一模一样才允许发 AJAX 请求

> 1. [http://baidu.com](http://baidu.com/) 可以向 [http://www.baidu.com](http://www.baidu.com/) 发 AJAX 请求吗    no
>
> 2. [http://baidu.com:80](http://baidu.com/) 可以向 [http://baidu.com:81](http://baidu.com:81/) 发 AJAX 请求吗   no



同源策略:  不是同域名不能向baidu发出ajax请求 **[只有ajax 不行]**

**突破同源策略 === 跨域**

-怎么访问 其他域名获取内容？ 打电话给后台 

1. 用JSONP 【JSONP不能post】   2. CORS 

CORS 可以告诉浏览器，我俩一家的，别阻止他  **设置响应头**

> `response.setHeader('Access-Control-Allow-Origin','http://frank.com:8001') `



Q : 为什么form 表单提交没有跨域问题，ajax 提交有：

A： 原页面用form 提交到另一个域名后，原页面的脚本无法获取新页面中的内容。

所以浏览器认为这是安全的。

而AJAX是可以读取相应内容的，因为浏览器不允许 这样不安全

其实上面 的请求都发出去了 ，只是拿不到响应

所以策略本质是： 一个域名的JS，在未经允许的情况下，不得读取另一个域名的内容。但浏览器不阻止你向另一个域名发送请求。

