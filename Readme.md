## node_demo :

		建立本地服务器  node 启动服务器软件 ： node server.js:8888
 				服务器文件(响应的部分)按照http 规则编写  :
						第一部分 协议/版本号 状态码 状态解释
	 					第二部分 key:value
						第三部分  \n
						第四部分  响应给客户端请求的内容

				客户端请求 http+url+port+查询路径 	
						根据服务器第二部分 Content-Type:类型（栗子：text/html）
						返回时浏览器按给的类型进行解析渲染，若没有给则默认纯文本格式。

 				通常请求index 根据传回来的html 一行行 如 link script img src 再去请求数据。	
				浏览器通过http协议 输入url 请求  通过 内网/ 路由器转发到 dns服务器解析 返回给出ip  再请求该ip 找到服务器
						查询需要的内容发出请求    经过tcp协议 三次握手后

			服务器软件根据请求内容进行处理  响应 返回内容给客户端 浏览器根据内容及类型进行渲染加载或请求。

### Resume
 	nav navigation 
 	a anchor  b bold 
    dl description list 
    	dt description term dd description definition
    div divided 
    ol orderd list li list item
    ul un-ordered
    alt alternate

 alt 写图片信息 防止遗忘
 div span 等标签加类名 知道它是干什么用的


 contenteditable
  空标签/可替换标签/  （wiki） MDN简介/w3c简介

  css  横向&纵向布局
  &amp;

  iframe  name属性 与 a target 属性一样  a的链接会在iframe中打开
  iframe 属性 iframeborder=0  消除自身边框
  a  target 值有 _blank _self _parent  _top   top（祖孙三代）parent (直接父亲) 在iframe内可运用
  a download 用于点击直接下载  法2 用http协议 Content-Type:application/octet-stream 指定该内容是下载而不是html
 a 标签 跳转页面（http  get请求）
1. a 标签的无协议绝对地址 //xxxx     该文件是什么协议 a跳转的就用什么协议
    npm i -g http-server    http-server -c-1 (参数是不要缓存) 
    		启动本地服务器  启动后 点击当前目录的html前面的协议会从file变为http  点击其内的a //qq.com 会跳转到qq
2. a 标签 href是相对地址 或 ‘’ 内写的字符串如 ？namesjdioasd 会添加到当前打开页面的url后面去访问。 
   当写 # 时不会发起get请求 。只有写锚点的时候才不发请求，因为锚点是页面的跳转
3. href  伪协议  href='javascript:alert(1);'   javascript:;

  form 标签 跳转页面（http  post请求）
  1. form 标签没有提交按钮是提交不了(除非Js)
  2. form 标签用于发出post请求    写get是把key放入查询部分而不是第四部分
  3. name 会被 带到第四部分作为 它的key
  4. 给action 后加入？12sd=asd之类 能让post 也有查询参数  get是没办法有第四部分的
  5. form target 与a 标签使用一样

		button  与 input 
如果一个form 只有一个button标签 且没写它的type
	它会自动升级 为提交按钮 默认为submit
	如果写了 type=button 那点了没反应
input 是按照写的type 来反应 button 则没反应 submit 则提交
	 	Sum:submit 是唯一来提交form表单的按钮 不是这个类型就是普通的按钮

 label 绑定 checkbox 等 提高体验
 老司机模式： label 直接将要关联的标签包起来也能一样效果

 checkbox 选中值默认(可以添加value来自定义)为on 未选择值没有 
 radio  同一个name 保证提交键值能获取值和只能选一个  checkbox 同name能选择多个

 select name  option disabled/selected  选中一个没有值的 就写value=''
 select 有multiple 可以多选

 textarea  css属性 resize:none 不让手动放大缩小  width：200px指定宽度和高 或者cols等 一般用css width精确控制

 table   colgroup( col width=100 控制一列的宽度 ) thead - tbody-tfoot  
 		table 内四个部分顺序无关 渲染显示按照标签部分规定的前后来显示
 collapse 



	# 3.CSS

​	94 .李爵士同事与伯特开发css   ----黑暗时代 标签/标签+属性 到 标签上的style属性内写样式---

​	标签style 内写 内联   外联link   及css文件内 @import  url(....css);

css2 2.1---css3   css 3 开始都是模块化升级  而不是整个css一起更新版本。

​	2.1 广泛  3 模块化  11年  

​	css　学习：MDN＋关键词　　／关键词＋spec   

​			 阮一峰+css /张鑫旭 css blog   

​			css  tricks  / codrops    /css secrets   

​    Target  :   引用--做---小---大----组织 CSS UI库

​	 知识点：  浮动  1：子元素都浮动  2 父元素 class = "clearfix"    

​		.clearfix { content:'' ; display:block; clear:both}

​		吸颜色： 颜色会是一个区间且每个os渲染不一样   、颜色获取：设计师/源码

​		下边框颜色  上边框也设置宽度 颜色为透明即可让元素不会hover 时抖动移位。



-----------------

### css  -2 

​	元素的高度由什么决定： 

​	1 块级元素高度 一般由其内部文档流元素的高度总和决定

​		文档流是： 文档内元素的流动方向 ：  内联元素 左到右 块级元素 上到下

 2. 内联元素高度 建议行高。

    ​	内联元素：中文理解 一个字是一个字 ；而英文是 一个单词时一个字 

     			所以一长串无间隔的字母不会换行。可以由css属性 word-break打断 粉碎性骨折：break-all

    ​		 能打断单词的字母让其换行显示

    ｄ：ｉｂ　快捷键　display:inline-block;

    字体大小 每个不一样 在不同浏览器渲染也不一，height不确定。但字体都是基线对齐的。



css bug  : 1 height  给固定值  ----会出问题。 如 子脱离 父高度会减少

​		 2  width:100%  加上Padding等，子宽度会 > 父    解决方式：子穿外套inner innner上加padding



背景图片 自适应：background-size : cover;

max-width : 940px ;  宽度大于940时自适应

居中： margin: 0 auto  ; text-align:center;

span  内联元素居中 用 padding .不用设置w/h .  最好再明确 line-height; 因为不同机器显示高度也会不一样。



画三角形： w:0  border:10px solid transparent; 三方向透明 其中一个给颜色 ---等腰直角三角形. 

​				border 某一边给w:0 则又是一个三角形。



绝对定位：脱标  子绝父相。 top:100%

csstricks shapes 查看css做出来的各种图行源代码

width1 40% width2 60% ----则父元素被子元素4 6 分



制作svg 字体图标：iconfont.com---购物车---symbol 生成链接 引入js----使用帮助symbol---复制粘贴

svg设置样式： {w/h: xx; 颜色：fill:white  平齐：vertical-align:top}     //行内元素顶部平齐

用padding 挤高度 用text-align 居中 用line-height在明确 居中。



头像：avatar

## css续

#### 一些概念

空标签： 没儿子的/自闭标签

类名 ：复用

id名： 身份证 唯一

伪类(nth-child(even)) 伪元素（before after 替代div 鼠标圈选不到伪元素）

​	nth-child(1)/first-child

  

#### 一些实现：

​		可以用来做太极图案 一个黑白半圆(渐变)组成的圆 ---内放两个黑白圆，黑白圆由空心的border宽度很大的圆组成。

#### css3 工具：	

​	渐变工具 搜索 css3 linear gradient generator

​	阴影工具 搜索 css shadow generator

​	动画效果：@keyframes spin{to :{} from:{}}  



#### 一些属性

过渡： transistion:box-shadow 1s;

一行俩技能条 ：padding+box-sizing:border-box ; 即可实现断层

margin-top  负值  让盒子上升，形成视觉上的定位？ 

​	与相对定位比较 不占用空间。与绝对定位比较 不脱标

点击下载 : a标签属性：download , href:url  target:_blank



#### 简写：

​	 text-align:center    ta:c

​	d:ib   一定要加 vec-align:top;   bug 底部会多空隙



### JS 状态机

​	逻辑上分析 几种状态：  例子  左中右三种 同时符合 

​	         接着写完该功能面子标签 后script标签内：  id0name.onclick=function(){ id2name.className="lei1 lei2"}.





​	



