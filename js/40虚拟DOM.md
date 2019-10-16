# Lesson 39   虚拟DOM

预览：

是什么？

> 模拟DOM 的映射  由 js --> dom结构的对象  

为什么？

> 一个***dom*****节点**属性非常多，针对***dom***的操作繁杂 内存消耗大。会`卡顿现象`
>
> 利用`虚拟dom`  ***js*** 先在**虚拟** ***dom***上操作，再将完成的结构　映射到 **真实DOM**上渲染

怎么实现？



比较修改前 虚拟dom    修改后 虚拟dom  差异 ----》局部渲染 --》映射

这样不用 全部重新渲染



## 一、简单实现

###  **数据改变   《=》 DOM 改变**



### Html

``` html
<div id="root"></div>
<button id="btn">change</button>
<scirpt src="./v-dom-es6.js"></scirpt>
```



### JS

``` javascript
//ES5 :
function vNode(tag,children,text){
    this.tag = tag
    this.text = text
    this.children = children
}

vNode.prototype.render = function(){
    if(this.tag === '#text'){
        return document.createTextNode(this.text)
    }
    let el = document.createElement(this.tag)
    this.children.forEach(vChild=>{
        el.appendChild(vChild.render())
    })
    return el
}

var node = new vNode('#text',[],'hello')
node.render()

```



#### ES6

``` javascript
//Demo 不考虑复杂情况

class VNode{
    constructor(tag,children,text){
        this.tag = tag
        this.text = text
        this.children = children
    }

    render(){
        if(this.tag === '#text'){
            return document.createTextNode(this.text)
        }
        let el = document.createElement(this.tag)
        this.children.forEach(vChild=>{
            el.appendChild(vChild.render())
        })
        return el
    }
}

function v(tag,children,text){
    if(typeof children === 'string'){
        text = children
        children = []
    }
    return new VNode(tag,children,text)
}


let vNodes = v('div',[
    v('p',[
        v('span',[
            v('#text', 'xiedaimala.com')])
    ]),
    v('span',[
        v('#text', 'jirengu.com')
    ])
])
console.log(vNodes.render())    // VNode {...}  

const root = document.querySelector('#root')
root.appendChild(vNodes.render())
```



### 更新  添加  删除 局部渲染 功能：

``` javascript
function patchElement(parent,newVNode,oldVNode,index = 0){
    if(!oldVNode){
        parent.appendChild(newVNode.render())
    }else if(!newVNode){
        parent.removeChild(parent.childNodes[index])
    }else if(newVNode.tag !== oldVNode.tag || newVNode.text !== oldVNode.text){
        parent.replaceChild(newVNode.render(), parent.childNodes[index])
    }else {
        for(let i=0;i<newVNode.children.length || i<oldVNode.children.length;i++){
            patchElement(parent.childNodes[index],newVNode.children[i],oldVNode.children[i], i)
        }
    }
}


let vNodes2 = v('div',[
    v('p',[
        v('span',[ 
            v('#text', 'xiedaimala.com')])
    ]),
    v('span',[
        v('#text', 'jirengu.com'),
        v('#text', 'sss'),
    ])
])

const root = document.querySelector('#root')
patchElement(root,vNodes2)
```

