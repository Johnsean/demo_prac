# Lesson 37 添加留言板 & MVC的M

js 两大功能 ： 1 DOM 操作 2 AJAX



用户 A   视图 V   逻辑【其他】C   数据M  服务器S

A 操作-> V      C监听 ->V    V 通知-> C  

C调用-> M  M请求 ->S  S响应->M  M返回给->C   C更新->V





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



## 设计留言板

### -css 追波找灵感 搜 message

``` html
<section>
    <h2> 留言</h2>
    <ol id='messageList'> </ol>
    <form id="postMessageForm" style="width:700px;margin:50px auto">
        <label> 姓名</labe>	
        <input type="text" name="name">
        <label>内容</label>
        <input type="text" name="content">
        <input type="submit" value="提交">
    </form>
</section>
section.message > ol {
    max-width:700px;
    margin:0 auto;
    border-top:1px solid #DDD;
}
section.message>ol>li{
    padding:16px;
    border-bottom:1px solid #DDD;
}
```

``` javascript
// 分别添加 
x.find().then((message)=>{
    //...
    li.innerText = `${item.name}: ${item.content}`
})

x.addEv('s',(e)=>{
    //...
    let name = myForm.querySelector('input[name=name]').value
    //...
    message.save({
        'name':name,
        'content':content
    })
}).then(()=>{window.location.reload()})
```

----本地直接添加

``` javascript
//上面👇
message.save({
    'name':name,
    'content':content
}).then((object)=>{
    let li = document.createElement('li')
    li.innerText = `${object.attributes.name}: ${object.attributes.content}`
    let messageList = document.querySelector('#messageList')
    messageList.appendChild(li)
    myForm.querySelector('input[name=content]').value = ''
})
```



---------------封装 

``` javascript
!function(){
    var view = document.querySelector('section.message')

    var controller ={
        view: null,
        messageList: null,
        init: function(view){
            this.view = view
            this.messageList = view.querySelector('#messageList')
            this.form = view.querySelector('form')
            this.initAV()
            this.loadMessage()
            this.bindEvents()
        },
        initAV: function(){
            var APP_ID = 'xxx'
            var APP_KEY = 'xxxx'
            AV.init({appId: APP_ID,appKey:APP_KEY})
        },
        loadMessages: function(){
            var query = new Av.Query('Message')
            query.find().this((message)=>{
                let array = message.map((item)=> item.attributes)
                array.forEach((item)=>{
                    let li = document.createElement('li')
                    li.innerText = `${item.name}: ${item.content}`      
                    this.messageList.appendChild(li) 
                })
            })
        },
        bindEvents: function(){
            this.form.addEventListener('submit',function(e){
                e.preventDefault()
                this.saveMessage()
            })
        },
        saveMessage: function(){
            let myForm = this.form
            let content = myForm.querySelector('input[name=content]').value
            let name = myForm.querySelector('input[name=name]').value
            var Message = AV.Object.extend('Message')
            var message = new Message()
            message.save({
                'name':name,
                'content':content
            }).then((object)=>{
                let li = document.createElement('li')
                li.innerText = `${object.attributes.name}: ${object.attributes.content}`
                let messageList = document.querySelector('#messageList')
                messageList.appendChild(li)
                myForm.querySelector('input[name=content]').value = ''
            })
        }
        controller.init(view)
}.call()
```

### ------------添加 M 

``` javascript
!function(){
    var view = document.querySelector('section.message')

    var model = {
        init: function(){
            var APP_ID = 'xxx'
            var APP_KEY = 'xxxx'
            AV.init({appId: APP_ID,appKey:APP_KEY})
        }, 
        fetch: function(){
            var query = new Av.Query('Message')
            return query.find()  // Promise 对象
        },
        //创建数据
        save: function(){
            var Message = AV.Object.extend('Message')
            var message = new Message()
            return message.save({ // Promise 对象
                'name':name,
                'content':content
            })
        }       
    }

    var controller ={
        view: null,
        model: null,
        messageList: null,
        init: function(view,model){
            this.view = view
            this.model = model
            this.messageList = view.querySelector('#messageList')
            this.form = view.querySelector('form')
            this.model.initAV()
            this.loadMessage()
            this.bindEvents()
        },
        loadMessages: function(){
            this.model.fetch().then((message)=>{
                let array = message.map((item)=> item.attributes)
                array.forEach((item)=>{
                    let li = document.createElement('li')
                    li.innerText = `${item.name}: ${item.content}`      
                    this.messageList.appendChild(li) 
                })
            })
        },
        bindEvents: function(){
            this.form.addEventListener('submit',function(e){
                e.preventDefault()
                this.saveMessage()
            })
        },
        saveMessage: function(){
            let myForm = this.form
            let content = myForm.querySelector('input[name=content]').value
            let name = myForm.querySelector('input[name=name]').value

            this.model.save(name,content).then(function(object){
                let li = document.createElement('li')
                li.innerText = `${object.attributes.name}: ${object.attributes.content}`
                let messageList = document.querySelector('#messageList')
                messageList.appendChild(li)
                myForm.querySelector('input[name=content]').value = ''
            })
        }

        controller.init(view,model)
}.call()
```

