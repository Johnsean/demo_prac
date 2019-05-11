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
