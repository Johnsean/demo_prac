# Lesson 27. DOM API

**DOM** :文档对象模型  *Model: Document* ---映射--> *Object* [内存]

> *DOM特指XML （html是xml的衍生品）==》dom api不是很好用*

> > *化腐朽为神奇： 把不好用的API 改为 好用的*

​        *DOM的主要功能：页面中的节点==构造函数(node)==》 对象*



###  1. 总

**DOM**是一棵树（***tree***）,树上有 **Node**

**Node** 分为 *Document*(html)   *Element*(元素)  *Text* (文本)  *Comment*(注释) 及其他不重要的

> JS 对象继承自Object
>
> 页面内的对象继承自 Node  【*Node继承自Object*】



*DOM标准*：对象存什么内容. 

> `console.dir(object);   ` 打印出该对象的所有属性和属性值.



### 2 .**Node** **的接口**

#### 1 .属性

*childNodes,firstChild,innerText,lastChild,nextSibling,nodeName,nodeType,nodeValue,outerText,ownerDocument,parentElement,parentNode,previousSibling,textContent*

> child / children / parent
>
> node  type
>
> first / last    next / previous
>
> sibling / siblings
>
> value / text / content
>
> inner / outer
>
> element

 然后互相组合



`childNodes`(包括回车这个文本节点) //返回的是**伪数组**   `children` 子标签

`nextSibling` & ` previousSibling`可能获得到的是文本节点

`nodeName` 输出为**大写**。**svg** 标签输出为小写，因为是外来标签。

`nodeType` 值为 **数字**。 历史原因 内存紧张

> **1 标签 3文本** 8 注释 9 html ... 
>
> 常量:形如Math.PI ------- Node.ELEMENT_NODE === 1



 无  ---->    IE  `innerText`      ---->   FF Opera: `textContent`  

**区别：**

> ***textContent*** 会获取所有元素的内容 包括 ***script & style*** 标签.
>
> ***textContent*** 会返回隐藏元素的文本。**innerText** 意识到样式不会返回
>
> **innerText** 受CSS↑↑样式影响，所以它会触发重排(*reflow*)
>
> IE version<=11 对**innerText** 修改，不仅会移出当前元素子节点，且永久性破坏所有后代文本节点

``` java
'textContent' in document.body ? document.body.textContent : document.body.innerText
```



####  2 .方法

（如果一个属性是函数，那么这个属性就也叫做方法；换言之，方法是函数属性）

> *appendChild()*
>
> ***cloneNode()***  // 深/浅[传入true/false]拷贝  深拷贝完全相同(包含子节点)，浅拷贝只拷最外层
>
> *contains()*
>
> *hasChildNodes()*
>
> *insertBefore()*
>
> ***isEqualNode()***  // 两节点是否相等
>
> ***isSameNode()***  //等价于 ===  俩节点是否是同一个
>
> *removeChild()* // 页面中移出(内存中被移除的元素还是存在的)
>
> *replaceChild()* //替换，同上
>
> ***[normalize](https://developer.mozilla.org/en-US/docs/Web/API/Node/normalize)()*** // 常规化。连续相邻文本节点(except 空白/回车)合为一
>
> ``` javascript
> var wrapper = document.createElement("div");
> 
> wrapper.appendChild(document.createTextNode("Part 1 "));
> wrapper.appendChild(document.createTextNode("Part 2 "));
> // 这时(规范化之前),wrapper.childNodes.length === 2
> // wrapper.childNodes[0].textContent === "Part 1 "
> // wrapper.childNodes[1].textContent === "Part 2 "
> 
> wrapper.normalize();
> // 现在(规范化之后), wrapper.childNodes.length === 1
> // wrapper.childNodes[0].textContent === "Part 1 Part 2"
> ```

---

DOM APi 无外乎「增删改查」



### 3 .**Document** **接口**

#### 1 .属性

>*anchors*  // h5 弃用
>
>*body*
>
>*characterSet*   // 字符编码 : 'UTF-8'
>
>*childElementCount*
>
>*children*
>
>*doctype*
>
>*documentElement*   //
>
>***domain***  //域名
>
>*fullscreen*
>
>*head*
>
>*hidden*
>
>*images*
>
>*links*
>
>***location***
>
>***onxxxxxxxxx***
>
>*origin*
>
>***plugins***  // 是否启用插件
>
>***readyState***
>
>***referrer***  // 引荐人，通过它来拒绝对其他网站提供服务/访问
>
>*scripts*
>
>*scrollingElement*
>
>*styleSheets*
>
> *title*
>
>*visibilityState*
>
>---

#### 2. 方法

>*close()*
>
>*createDocumentFragment()*
>
>*createElement()*
>
>*createTextNode()*
>
>***execCommand()***
>
>*exitFullscreen()*
>
>*getElementById()*   //以下四个不用 有queryxxxx
>
>*getElementsByClassName()*
>
>*getElementsByName()*
>
>*getElementsByTagName()*
>
>*getSelection()*
>
>*hasFocus()*
>
>*open()*
>
>***querySelector()***    //element 也有这两个方法	
>
>***querySelectorAll()***  //伪数组   instanceof array  /false
>
>*registerElement()*
>
>*write()*    // open -> write - >close  避免在异步函数内使用。文档加载完会close,异步导致覆盖
>
>*writeln()*  // 写完回车/由于html显示回车为缩进而不是换行
>
>---



### 4. **Element** **的接口**

见 MDN

`innerText` & `innerHTML` 

innerHTML 有安全问题[可以写入script脚本获取Cookies等信息] 虽然浏览器有防止机制。

### 5 . **DOM API** **反人类**

>1. 获取元素  ：以前之后 document.getElementById, document.getElementsByTagName, document.getElementsByClassName
>   太反人类，于是有了 jQuery
>   后来 DOM API 终于抄袭 jQuery 提供了 document.querySelector 和 document.querySelectorAll
>   但是依然没有 jQuery 好用，因为「不一致」
>
>1. 获取下一个元素
>
>2. 获取兄弟们
>



