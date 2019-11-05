# Lesson 46 从MVC 到　MＶＶＭ

1. vue + axios

> axios:`response.data`  `mock 数据`  ` interceptors  `  `response.config`  `data,url,method`
>
> `object.assign(obj,obj2)`
>
> vue

2.mvc 

3.mvvm



## 一、 axios

### axios 库  **vs** jQuery 

***jQuery*** [ $.ajax  $()  $().addClass()... ]    /  `ajax  + dom `等的功能

***axios***   [put delete patch...] ---- 1. 功能更多 （api 比jq.ajax 的多）2. 更专一 只有ajax功能 

***vue*** 来负责 dom  部分即可   

--------引用 ***Vue &axios*** **[职责分明]** 即可 不用再用  **jq**

---

### axios 的劫持

路由时 `axios.get('/book/1').then((response)=>{console.log(response)})`  怎么造数据？

``` javascript
// 两种劫持： 1. 直接 放在data 里  2. 通过url不同赋不同值  response.config

// 在真正返回 response 之前使用
axios.interceptors.response.use(function(response){
    //1. 这里可以直接写死 response  mock数据  =》 response.data={'name':'bob'}

    let {config:{method,url,data}} = response  //2. data 是请求的data
    if(url === '/book/1' && method === 'get'){  
        response.data = {  //data 是响应的data 即响应第四部分
            name:'frank',number:2,id:1
        }
    }

    return response
})
```

``` html
<body>
    <div id='app'>
        <div>
            书名： 《__name__》
            数量： <span id=number>__number__</span>
        </div>
        <div>
            <button id='addOne'>加1</button>
            <button id='minusOne'>减一</button>
            <button id='reset'>归零</button>
        </div>
    </div>
</body>
```



### $.html() 清空利用委托

``` javascript
axios.get('/book/1')
    .then(({data})=>{
    let originalHtml = $('#app').html  //初始html  初始app内有 文本 和 按钮
    let newHtml = originalHtml.replace('__name__',data.name)
    .replace('__number__',data.number)
    $('#app').html(newHtml) //html()后 会使app 内按钮替换新的按钮 其点击事件失效
})

// 利用委托 避免按钮重置 使事件失效
$('#app').on('click','#appOne',function(){ //#minusOne #reset
    var oldNumber =$('#number').text //string
    var newNumber = oldNumber - 0 +1
    axios.put('/book/1',{  //上传number  对应下面assign部分的代码
        number:newNumber
    }).then(()=>{
        $('number').text(newNumber)  //成功则替换
    })
})
```



### Object.assign() 局部更新

obj.assign(obj,data)  部分更新。将data 数据往obj中覆盖

``` javascript
//EX: 使用：
var book = {name:'fos',number: 1, id: 2}
Object.assgn(book, {name:'f'}, {number: 2})
	//book: {name:'f',number: 2, id: 2}
```



``` javascript
let book = {
    name:'jS高程',
    number: 2,
    id: 1
}
//劫持前的声明 👆👆👆

//劫持部分 路由
else if(url === '/book/1' && method === 'put'){   // put 时
    data = JSON.parse(data)
    Object.assign(book,data)
    response.data = book
}

/* 上面是假的后台 */
```





二、  用 MVC 改写代码  

