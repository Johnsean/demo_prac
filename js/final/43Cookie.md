# Lesson 43 Cookie  登陆注册

响应头 ： ***cookie***  /  ***cache-control***  [cash 同音]

cookie **缺点**： 

> 1.必附http 头  2. 明文传递  3. 限制大小4KB左右



**特点**

> 1. 服务器通过 ***Set-Cookie*** 响应头设置 ***Cookie***
> 2. 浏览器得到 ***Cookie*** 之后， 每次请求都要带上 ***Cookie***
> 3. 服务器读取 ***Cookie*** 就知道登陆用户的信息（email）



注册： 写入数据库 

> 1. 打包数据  发送服务器
>
> 2. 解析数据，服务器验证  后 与数据库对比，不重复则写入
> 3.  返回 是否注册成功 

登陆： 与 数据库对比  做标记给cookie

> 1. 打包数据  发送
> 2. 解析数据， 并与数据库对比 
> 3. 返回 ， 成功则 响应 设置 cookie头 跳转首页
> 4. 跳转首页  分解请求头  `request.headers.cookie`根据 得到的cookie  返回相应的 用户html界面



### 注册 js

``` javascript
let hash = {}
$('#signUpForm').on('submit',(e)=>{
    e.preventDefault()
    let need = ['email', 'password', 'password_confirmation']
    need.forEach((name)=>{
        let value = $('#signUpForm').find(`[name =${name}]`).val()
        hash[name] = value
    })
    
    
    $.post('/sign_up', hash)    
        .then(()=>{
        console.log('success')
    }, ()=>{
        console.log('error')
    })
})
```



### 后台Nodejs 解析传递的数据

``` javascript
// node http get post data
let body = []  // 请求体
request.on('data',(chunk)=>{
    body.push(chunk)
}).on('end',()=>{
    body = Buffer.concat(body).toString()
    console.log(body)
    response.statusCode = 200
    response.end()
})

//以上封装
function readBody(request){
    return new Promise((resolve,reject)=>{
        let body = []  // 请求体
        request.on('data',(chunk)=>{
            body.push(chunk)
        }).on('end',()=>{
            body = Buffer.concat(body).toString()
            resolve(body)
        })
    })
}

//传递的解析出来 
else if(path === '/sign_up' && method ==='POST'){
    readBody(request).then((body)=>{
        let strings = body.split('&') //['emali=1','passowrd=2', ...]
        let hash = {}
        strings.forEach((string)=>{
            //string == 'email=1'
            let parts = string.split('=')  //['email', '1']
            let key = parts[0]
            let value = parts[1]
            hash[key] = decodeURIComponent(value) // hash['email'] = '1'
        })
        console.log(hash)    // {email:'1', ...}
        
        let {email,password,password_confirmation} = hash
        if(email.indexOf('@') === -1){
            response.statusCode = 400
            response.setHeader('Content-Type','application/json;charset=utf-8')
            response.write(`{
                "error":{
                "email":"invalid"
                }
			}`)
        }else {
         // 正常情况  写入数据操作
            var users = fs.readFileSync('./db/users', 'utf-8')
            try{
                users = JSON.parse(users) //[]
            }catch(exception){
                users = []
            }
            let inUse = false
            for(let i=0;i<users.length;i++){
                let user = users[i]
                if(user.email === email){
                    inUse = true
                    break;
                }
            }
            if(inUse){
                response.statusCode = 400
                response.write('email in use')
            }else{
                users.push({email: email, password: password})
                var usersString = JSON.stringify(users)
                fs.writeFileSync('./db/users', usersString)
                response.statusCode = 200
            }
        }
        response.end()
    })
}
```



### 前台

``` javascript
let $form = $('#signUpForm')

//   .  .  .

$form.find('.error').each((index, span)=>{
    $(span).text('')
})

if(hash['email'] === ''{
   $form.find('[name="email"]').siblings('.error').text('填邮箱呀')
	return
   })
 // 等等...

$.post('/sign_up', hash)    
    .then((response)=>{
    console.log(response)
}, (request)=>{
    // let {errors} = JSON.parse(request.resonseText)
    let {errors} = request.responseJSON

    if(errors.email && errors.email === 'invalid'){
        $form.find('[name="email"]').siblings('.error').text('邮箱格式错误')
    }
    console.log('error')
})
```



### 登陆 路由 后台

``` javascript
else if(path === '/sign_in' && method ==='GET'){
    let sting = fs.readFileSync('./sign_in.html','utf-8')
    response.statusCode = 200
    response.setHeader('Content-Type','text/html;charset=utf8')
    response.write(string)
    response.end()
}else if(path === '/sign_in' && method ==='POST'){
    readBody(request).then((body)=>{
        let strings = body.split('&') //['emali=1','passowrd=2', ...]
        let hash = {}
        strings.forEach((string)=>{
            //string == 'email=1'
            let parts = string.split('=')  //['email', '1']
            let key = parts[0]
            let value = parts[1]
            hash[key] = decodeURIComponent(value) // hash['email'] = '1'
        })
        let {email,password} = hash
        var users = fs.readFileSync('./db/users', 'utf-8')
        try{
            users = JSON.parse(users) //[]
        }catch(exception){
            users = []
        }
        let found 
        for(let i=0;i<users.length;i++){
            if(users[i].email === email && users[i].password === password){
                found  = true
                break;
            }
        }
        if(found){
            response.setHeader('Set-Cookie',`sign_in_email=${email}`)
            response.statusCode = 200
        }else{
            response.statusCode = 401
        }
        response.end()
```

``` javascript
//前台
$.post('/sign_in',hash).then((response)=>{
    window.location.href = '/'
},(request)=>{ 
    //...
})

//路由 
if(path ==='/'){
    let strings = fs.readFileSync('./index.html', 'utf-8')
    let cookies = request.headers.cookie.split('; ')
    let hash = {}
    for(let i=0;i<cookies.length;i++){ 
        let parts = cookies[i].split('=') 
        let key = parts[0]
        let value = parts[1]   
        hash[key] = value
    }
    let email = hash.sign_in_email
    let users = fs.readFileSync('./db/users', 'utf8')
    users = JSON.parse(users)
    let foundUser
    for(let i=0;i<users.length;i++){ 
        if(users[i].email === email){
            foundUser = users[i]
            break;
        }
    }
    if(foundUser{
    	string = string.replace('__password__',foundUser.password)
	}else{
    	string = string.replace('__password__','不知道')
	}
}
```

