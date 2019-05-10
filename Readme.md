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
   
 alt 写图片信息 防止遗忘
 div span 等标签加类名 知道它是干什么用的