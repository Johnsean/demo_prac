# Lesson 44 Session 

`LocalStroage`  `SessionStroage(会话存储)`  `Cache-Control` `Expire`  `Etag` `302`

预览： 401 没权限访问  304不下载

***cookie*** 存在c盘， 附在`HTTP头`上。

> 暴露帐号，修改cookie可以切换用户 安全风险 

↑规避风险使用 ***session*** ： 服务器上一块内存 (哈希表)

> 利用随机数规避暴力穷举。通过session[key随机数] 取得对应的 帐号相关信息访问

***LocalStroage*** 存在c盘  h5提供的api

> 持久化存储，存储为字符串 调用toString方法，存对象应用stringify
>
> ***.setItem()  .getItem()   .clear(***)清空

***SessionStroage*** 使用同 LocalStroage   。与session 无关系

> 会话结束清空



***Cache-Control***  缓存控制  设置失效时间长度 。 失效前 不会发请求而是用本地缓存的数据

> max-age=121212   多少秒后过期

在失效之前，可通过更改 url  来更新最新的版本

> 1. 通过查询参数  `<link src="xx.js?v=2"/>` 
> 2. 通过文件名 如使用 MD5(webpack)  `xx.asdah2323.js`

***Expires***  什么时候过期 。 设置失效时间点  （本地时间点）  

> 优先使用 Cache-Control



`last modified`

`md5` 摘要算法  确认文件下载正确

> 根据文件内容生成的值，内容差异越小，值差距越大（放大式）

***Etag***   请求 但是 若没差异 则不下载   （与Cache-Control 对比）

> 用来给版本号    302没有更改



## 一、session

​	***cookie*** : 1. Set-Cookie  2. 每次请求相同域名是带上Cookie

``` javascript
//cookie 时代码  暴露自己的 email
if(found){
    response.setHeader('Set-Cookie',`sign_in_email=${email}`)
    response.statusCode=200
}
```

​	换成给 ***sessionId***  , sessionId 是个随机数  里面存email

``` javascript
// 利用session 改为
let sessions = {}
//. . . .
if(found){
    let sessionId = Math.radom()*100000
    sessions[sessionId] ={sign_in_email: email}
    response.setHeader('Set-Cookie',`sessionId=${sessionId}`)
    response.statusCode=200
}else{
    response.statusCode=401 //没授权
}

//此时  if(path ==='/')  ...:
let email = hash.sign_in_email

//改为     
let mySession = sessions[hash.sessionId]
let email
if(mySession){  // mySession 是个内存 一关闭就没有了 要判断：
    email = mySession.sign_in_email
}
```

-cookie 是告诉 你是谁(帐号)

-session 是给随机数key(票)给你  你把随机数告诉我，我就知道你是谁

> 服务器通过 cookie 给用户一个 sessionId
>
> sessionId 对应服务器中的一小块内存（hash）
>
> 每次访问服务器通过sessionId 读取对应的 session 来知道用户的隐私信息
>
> ​		安全由随机数保证（很难猜） 如23131.1231546546



### 面试题 

Cookie 4k

``` javascript
1. 服务器通过 Set-Cookie 头给客户端一串字符串
2. 客户端每次访问相同域名的网页时，必须带上这段字符
3. 客户端要在一段时间内保存这个Cookie
4. Cookie 默认在用户关闭页面后就失效，后台代码可以任意设置 Cookie 的过期时间
```

Session (不翻译)   缺点：占内存

``` javascript
1. 将 SessionId(随机数) 通过 Cookie 发给客户端
2. 客户端访问服务器时， 服务器读取 SessionId
3. 服务器有一块内存（哈希表）保存了所有 session
4. 通过 SessionId 我们可以得到对应用户的隐私信息，如id ,email
5. 这块内存（哈希表）就是服务器上的所有 session
```



## 二、LocalStroage

localStorage  是 h5 提供的API      实质是浏览器上的hash

 cookie  是http协议内容          session 实质是 服务器上的hash

``` javascript
localStorage.setItem('jsonString', JSON.stringify({name:'obj'}))
localStorage.getItem('jsonString')  //"{"name":"obj"}"
localStorage.clear() // 清空所有LS
```

### 	使用： -最典型的使用： 跨页面

 		localStorage  & js 变量区别： js变量 页面关闭死掉，刷新重置。

> localStorage 存在c 盘文件 持久化存储

``` javascript
let a = localStorage.getItem('a')
if(!a){
    a=0
}else{
    a=+(a)+1
}
console.log('a');console.log(a)  //0 1 2 3...
localStorage.setItem('a',a)
```



​	 **跨页面**  :  只提示一次信息  再次不提示

``` javascript
let already = localStorage.getItem('已经提示了')
if(!already){
    alert('你好，我们网站已经改版了，有xxx功能')
    localStorage.setItem('已经提示了',true)
}
```

### 特点:

``` javascript
1. LocalStorage 跟 HTTP 无关
2. HTTP 不会带上 LocalStorage 的值
3. 只有相同域名的页面才能互相读取 LocalStorage (没有同源那么严格)
4. 每个域名 LocalStorage 的最大存储量为 5Mb左右 （每个浏览器不一样）
5. 常用场景：记录又没有提示过用户（没有用的信息，不能记录密码等）
6. LocalStorage 永久有效，除非用户清理缓存
```

SessionStorage (会话存储)

``` javascript
API 也同上使用
1、2、3、4 同上
5. SessionStorage 在用户关闭页面（会话结束）后就失效
```

#### 测试存储量：

``` javascript
let s = ''
for(let i=0;i<10000;i++){  //改变i 来测大小
    s +='一二三四五六七八'
}
console.log(s.length) //8w 长度 8(位)*2（字节）*8w/1024=·1Mb 
localStorage.setItem('xxx', s)
```

---

### 面试题：  

cookie  session  关系： 一般来说session 基于（依赖）cookie 实现的

cookie localStorage 区别： 前者：`http带上  4k`   后者： `http不带上 5M`

> 因为历史原因  localStroage 是新api 之前是利用 cookie来做跨页面持久化存储
>
> cookie 也是存储在 c 盘  /   每次请求都会把cookie 带上 请求传输就特别大
>
> 前端永远也不要读写 cookie

localstorage sessionstorage 区别：  前者永久有效  后者关闭页面(会话结束)失效



### 超纲内容： session 可不基于 cookie 实现

通过 查询参数 和 localStorage 存储实现：

``` javascript
//前端
$.post('/sign_in',hash)
    .then((response)=>{
    //sessionId 存到 localStorage
    let object = JSON.parse(response)
    localStorage.setItem('sessionId',object.sessionId)
    window.location.href = `/?sessionId=${object.sessionId}`
},(request)=>{
    alert('不匹配')
})

//后台
    response.setHeader('Set-Cookie',`sessionId=${sessionId}`)
// 改为
	response.write('Set-Cookie',`sessionId=${sessionId}`)

//let mySession = sessions[hash.sessionId]
//改为：
    let mySession = session[query.sessionId]  //add this lines
```



## 三、HTTP缓存 

cache control mdn `google`

``` javascript
if(path === '/js/main.js'){ ...
response.setHeader('Content-Type','application/javascript;charset=utf8')
response.setHeader('Cache-Control','max-age=30')
    ...
```

请求后 再次请求时：30s 内不会再次请求了  这是最新的

> 首页 设置这个头 不会生效成功 、因为有风险  用户获取不到最新网页
>
> 一般 html 都不设置这个头   而css js 设置10年   => 3e 秒



若 js / css  变了 -------------我们可以通过更改 ***url***  来更新最新的 js css 

通过 加 查询参数 来更新 url  

或者 文件名md5 《通过webpack》



### Expires  

cache control 头 优先级高于 它

``` javascript
d= new Date()
d.toGMTString()  // Expires 格式的时间戳
```

与cache control  区别：一个是多长时间过期  一个是  什么时候过期 

Expires 使用的是 本地时间----本地时间过期



### Etag

`md5` 摘要算法  值：微小差异放大

nodejs md5 `google`

``` javascript
if(path === '/js/main.js'){
    let string = fs.readFileSync('./js/main.js','utf8')
    response.setHeader('Content-Type','application/javascript;charset=utf8')
    var md5 = require('md5')
    let fileMd5 =md5(string)
    response.setHeader('Etag',fileMd5)
    if(request.headers['if-none-match'] === fileMd5){
        //没有响应体
        response.statusCode = 304  //没改过  请求了但不下载
    }else {
        // 有响应体
        response.write(string)  //170k
    }
    response.end() 
    ...
```

再次刷新  多了个请求头`If-None-Match` 会把上个md5 放进来：

看是否 改过  没改则不下载



Etag  :  请求 直接不下载 用缓存

Cache-Control   直接不请求  用缓存   //这个更好  更省时间  请求都不去做