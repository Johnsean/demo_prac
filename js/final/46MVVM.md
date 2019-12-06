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





## 二、  用 MVC 改写代码  

``` javascript
function fakeData(){
    let book = { 
        name:'jS高程',
        number: 2,
        id: 1
    }
    axios.interceptors.response.use(function(response){
        let {
            config:{
                method,url,data
            }
        } = response  

        if(url === '/book/1' && method === 'get'){ 
            response.data =book
        }else if(url === '/book/1' && method === 'put'){   
            data = JSON.parse(data)
            Object.assign(book,data)
            response.data = book
        }
        return response
    })
}

fakeData()  //初始化
```

**Model:**

``` javascript
let model = {
    data: {
        name:'',
        number:0,
        id:''
    },
    fetch(id){
        return axios.get(`/book/${id}`).then((response)=>{
            this.data = response.data
            return response
        })
    },
    update(data){
        let id = this.data.id
        return axios.put(`/book/${id}`,data).then((response)=>{
            this.data = response.data
            return response
        })
    }
}
```



**View:**

``` javascript
let view = {
    el: '#app',
    template:`
        <div>
        书名： 《__name__》
        数量： <span id=number>__number__</span>
        </div>
        <div>
            <button id='addOne'>加1</button>
            <button id='minusOne'>减一</button>
            <button id='reset'>归零</button>
        </div>
	`,
    render(data){
        let html = this.template.replace('__name__',data.name)
        .replace('__number__',data.number)
        $(this.el).html(html) 
    }
}
```



**controller:**

``` javascript
var controller = {
    init(options){
        let view = options.view
        let model = options.model
        this.view = view
        this.model = model
        
        this.view.render(this.model.data)
        this.bindEvents()
        
        this.mode.fetch(1).then(()=>{
            this.view.render(this.model.data)
        })},
    addOne(){
        var oldNumber =$('#number').text //string
        var newNumber = oldNumber - 0 +1
        this.model.update({number:newNumber}).then(()=>{
            this.view.render(this.model.data)
        })
    },/...
    bindEvents(){
        $(this.view.el).on('click','#addOne',this.addOne.bind(this))
        // ...etc
    }
}
controller.init({view:view,model:model})
```



### 共有属性做：

MV

``` javascript
function Model(options){
    this.data = options.data
    this.resource = options.resource
}
Model.prototype.fetch = function(id){
    return axios.get(`/${this.resource}s/${id}`).then((response)=>{
        this.data = response.data
        return response
    })
}

Model.prototype.update= function(data){
    let id = this.data.id
    return axios.put(`/${this.resource}s/${id}`,data).then((response)=>{
        this.data = response.data
        return response
    })
}

function View({el, template}){
    this.el =el
    this.template =template
}
View.prototype.render(data){
    let html = this.template
    for(let key in data){
        html = html.replace(`__${key}__`,data[key])
    }
    $(this.el).html(html) 
}
```

``` javascript
//----------------- 上面是 MVC 类 ，下面是对象
let model = new Model({
    data: {
        name: '',
        number: 0,
        id: ''
    },
    resource: 'book'
})

let view = new View({
    el: '#app',
    template:`
        <div>
        书名： 《__name__》
        数量： <span id=number>__number__</span>
        </div>
        <div>
            <button id='addOne'>加1</button>
            <button id='minusOne'>减一</button>
            <button id='reset'>归零</button>
        </div>
	`
})
```

C 暂不改



## 三、用 Vue 改写

mv[-更智能]c[-可合并到v]---自动化---> mvvm



Vue 定位

> m -  ajax (fetch update)
>
> v -  dom (el)
>
> c -  数据结构/算法/面向对象等知识

---

改写只用改 view ---> vue    还可以将 c 加入 vue

vue中：

> ***data*** 必须有  初始化

> ***template***  只识别一个根元素 所以多个的时候外面套一个div 即可

> `__ke__` 改为用{} 包起的语法
>
> ***render*** 就不需要做了 Vue 会帮我们做

``` javascript
let view = new Vue({
    el: '#app',
    data: {
        name: '未命名',
        number: 0,
        id: ''
    },
    template:`
	<div>
        <div>
        书名： 《{{name}}》
        数量： <span id=number>{{number}}</span>
        </div>
        <div>
            <button id='addOne'>加1</button>
            <button id='minusOne'>减一</button>
            <button id='reset'>归零</button>
        </div>
	</div>
	`
})

//c : fetch 时 在view上面操作： 而不是this.view.data=this.model.data.name
this.mode.fetch(1).then(()=>{
    this.view.name = this.model.data.name
    this.view.number = this.model.data.number
    this.view.id = this.model.data.id
})},
```



 view  只更新该更新的地方（局部更新）  ,而我们之前的mvc例子 是更新了三个div



### Vue  上 绑定事件  （controller 就不需要了）

``` javascript
let view = new Vue({
    el: '#app',
    data: {
        book:{
            name: '未命名',
            number: 0,
            id: ''}
    },
    template:`
	<div>
        <div>
        书名： 《{{name}}》
        数量： <span id=number>{{number}}</span>
        </div>
        <div>
            <button v-on:click='addOne'>加1</button>
            <button v-on:click='minusOne'>减一</button>
            <button v-on:click='reset'>归零</button>
        </div>
	</div>
	`,
    created(){   // 去掉controller 记得初始化
        this.mode.fetch(1).then(()=>{
            this.book = model.data
        })
    }
    ,
    method:{
        addOne(){
            model.update({
                number: this.book.number+1
            }).then(()=>{
                this.view.book = this.model.data
            })
        }, // ...
    }
})
```



### 双向绑定： 

单向：  内存改变 ---vue --> 页面显示数据变化 { n }

双向:   内存改变 ---vue--> input 显示改变  | input-value改变 ---> 内存改变

``` javascript
data : { n:1,book:{name:'1',number:2，id:''}}
`{{book.name}}
{{book.number}}
<input v-model="n"/> 
    N的值是 {{n}}
<button v-on:click="addOne">加N</button>
`
```



### 四、Vue 三个demo

`script : axios + vue`

`<div id="app"> </div>`

### demo1：[toggle](http://js.jirengu.com/tetub/6/edit?html,css,output)

``` javascript
let view = new Vue({
    el: '#app',
    data:{
        open: false
    },
    template:`
        <div>
            <button v-on:click="toggle">点我</button>
            <div v-if="open">你好</div>
        </div>
        `,
    methods:{
        toggle(){
            this.open=!this.open
        }
    }
})
```



### Demo2 :[轮播](http://js.jirengu.com/yujug/5/edit?html,output)

``` css
.sliders{
    width: 300px;
    background: red;
    height: 100px;
    transition: all 1s;
}
.window{
    width: 100px;
    height: 100px;
    border: 1px solid black;
}
```

``` javascript
let view = new Vue({
    el: '#app',
    data: {
        tranformValue: '',
        x:['/x','/y','/z'] //添加图片地址
    },
    template:`
    <div>
        <div class="window">
            <div class="sliders" 
            :style="{transform:tranformValue}">
				<img v-for="url in x" :src="url" height="100">
			</div>
        </div>
        <button v-on:click="go(0)">1</button>
        <button v-on:click="go(1)">2</button>
        <button v-on:click="go(2)">3</button>
    </div>`,
    methods: {
        go(index){
            this.transformValue = `translateX(${-100*index}px)`
        }
    }
})
```



### Demo3: [选中显示内容](http://js.jirengu.com/daloc/1/edit?html,output)

``` javascript
let view = new Vue({
    el: '#app',
    data: {
        selected: 0
    },
    template:`
<div>
    <ol> 
        <li :class="{active:selected === 0}" @click="selected = 0">1<li>
        <li :class="{active:selected === 1}" @click="selected = 1">2<li>
        <li :class="{active:selected === 2}" @click="selected = 2">3<li>
    </ol>
    <ol> 
        <li v-show="selected === 0}">1<li>
        <li v-show="selected === 1}">2<li>
        <li v-show="selected === 2}">3<li>
    </ol>
</div>`,
    methods: {
    }
})
```



v-for:

``` javascript
let view = new Vue({
    el: '#app',
    data: {
        selected: 'a',
        tabs:[
            {name: 'a',content: 'aaa'},
            {name: 'b',content: 'bbb'},
            {name: 'c',content: 'ccc'}
        ]
    },
    template:`
<div>
    <ol> 
    	<li v-for="tab in tabs"  
            @click="selected =tab.name"
            :class="{active:tab.name === selected}"
		>{{tab.name}}<li>
    </ol>
    <ol> 
    	<li v-for="tab in tabs" 
        	v-show="selected === tab.name"
        >{{tab.content}}<li>
    </ol>
</div>`,
    methods: {
    }
})
```

