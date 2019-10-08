# Lesson 37 添加留言板 & MVC的M

js 两大功能 ： 1 DOM 操作 2 AJAX



留言板：

Q:换设备浏览 能看到提交后的留言

A: 数据存到必须服务器上 【 storage 只能本地浏览器缓存 换设备看不到 】



使用公共的数据库 https://leancloud.cn 注册账户。

**LeanCloud** 它是自带数据库和CRUD的后台系统

---创建引用 resume  --创建 Class  [表名]

--- CRM 即可

1.引入CDN 2 console.log(AV) 3. ping 4 copy 5 modify

初始化AV对象， 新建一条数据



留言 **提交** 绑定事件 用 ***submit*** 而不是click 

> 有时候用户会 使用回车来提交  若使用点击 代码还要判断 按下事件
>
> ``` javascript
> let button = myForm.querySelector('input[type=submit]')
> button.addEventListener('click',function(){})
> let input =myForm.querySelector('input[name=content]')
> input.addEventListener('keypress',function(e){
>     if(e.keyCode === 13){...}
> })
> ```



### 存数据库：

``` html
<section>
    <h2> 留言</h2>
    <form id="postMessageForm" style="width:700px;margin:50px auto">
        <input type="text" name="content">
        <input type="submit" value="提交">
    </form>
```

``` javascript
let myForm = document.querySelector('#postMessageForm')

myForm.addEventListener('submit',function(e){
    e.preventDefault()
    let content = myForm.querySelector('input[name=content]').value
    let Message = AV.Object.extend('Message')
    var message = new Message()
    message.save({
        'content':content
    }).then(function(object){
        alert('存入成功')
        console.log(object)
    })
```

## 展示出来：

``` javascript
<ol id='messageList'> </ol>
var query = new AV.Query('Message')
query.find()
    .then(
    function(message){
        let array = message.map((item)=> item.attributes)
        array.forEach((item)=>{
            let li = document.createElement('li')
            li.innerText = item.content
            let messageList = document.querySelector('#messageList')
            messageList.appendChild(li)
        })
})
```

