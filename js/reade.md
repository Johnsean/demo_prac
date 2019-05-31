## JS 目录 ##



### [demo-1](//https://johnsean.github.io/demo_prac/js/demo-1/) ： 

&emsp;&emsp;&emsp;&emsp;**按键跳转网页/搜索操作  | 增加自定义按钮跳转**

**进阶版**  : 封装代码成函数=》 逻辑关系近的放一起，远的隔行开 =》局部封装  =》整体封装----三步走 数据-创建-监听。



---

## Details

#### &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;**demo-1**:

&emsp;*实现思路* - &emsp;&emsp;用户进行按键操作 触发事件 事件进行相应跳转&emsp;--—— 获得按的哪个键，跳转到相应界面。

**准备工作**：

&emsp;*1.&emsp;用例图*：用户A --》[举例]：进行按键 a-z ,会在新窗口打开相应界面。

&emsp;*2.&emsp;数据结构*：需要按键及其相应的值：用 **hash.**  &emsp;&emsp;友好性：得有按键的图像。---将按键插入到页面中.比手打效率。

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;按键盘的格式显示 分为3行 则用3个数组存===》数组：值 ===》也可以用 **hash**

&emsp;3.&emsp;书写代码。 详见代码。 

**分析**：1. 用 js 将按键【hash形式】写入 html中  &emsp;&emsp;2. 监听按键 判断 打开相应网站

3. 添加功能：自定义网站。 ==》1.&emsp;写入html时同时内部写入Btn 。2.&emsp;监听btn点击事件 3.&emsp;修改相应的【hash值】

   具体方式===》点击==》弹框==》获取用户输入==》覆盖相应hash。

   &emsp;&emsp;&emsp;4.&emsp;页面刷新会重置初始化【hash】，因为会重新执行 JS .避免【hash】被覆盖，使用浏览器来存储该值==》localStorage == 》刷新页面的时候将 浏览器中存储**hash**取出覆盖**js**初始变量
> **用到的知识**： `localStorage` /`Json解析&转化`/`监听事件`/`创建文档节点`/`hash`/`css字母转大写`
>
> ```javascript
> localStorage.setItem('lsname','string'); localstorage.getItem('lsname');
> JSON.parse('json') =>object ; JSON.stringify(object) =>'string'
> document.createElement('div') ; namediv.appendChild(namespan);
> namediv.textContent='textcontent' // window.open(url,'_blank');location.href=url;
> css: text-transform: uppercase;
> //监听 取按键 取id  function(asd){}
> asd['key'];asd['target']['id'];
> ```
>
> ---
>
> 
