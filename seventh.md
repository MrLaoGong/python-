#学习使用Fiddler
###Fillder捕获会话功能  
在火狐浏览器点击打开菜单，选择选项，在弹出的页面点击高级，在连接栏中点击设置，在连接设置对话框中，设置手动配置代理，在HTTP代理中填写，127.0.0.1，在端口中写，8888,单击确定.  
Fiddler设置能够捕获HTTPS的会话信息。
打开Fidder，单击“Tools->Fiddler Options",选择HTTPS选项卡,勾选
Capture HTTPS CONNECTS,....from all processes,Decypt HTTPS traffic,Ignore server certificate,Check for certificate revocation,单击确定。  
配置好之后，Fiddler就可以捕获浏览器与服务器之间的HTTP和HTTPS会话信息了。打开任意一个网址，在右的界面中，有一个标签“Statistics”，显示的是一些页面统计信息。  
将标签切换为“Inspectors”，显示的是一些嗅探信息，如Headers等。  
###使用QuickExec命令行   
1. cls  
cls命令为清屏命令，输入该命令可以清空会话列表中的所有会话。
2. select  
通过select命令我们可以选择出某一类型HTTP会话的功能，比如像选择出所有的html网页类型的HTTP会话，可以输入命令:
	select html
3. ?  
?命令可以查找出网址中包含某些字符的会话信息，比如“？data2"可以查找出网址中包含”data2“字符串的会话信息都被筛选了出来，用不同颜色区分。 
4. help  
输入help打开官方使用手册。  
###Fiddler断点功能
Fiddler是作为代理服务器的方式进行工作的，所以本地应用与服务器传递的这些数据都会经过Fiddler，有的时候，我们希望在传递的中间进行修改后再传递，那么可以使用Fiddler断电功能。  
	1. 拦截相应数据，并进行相应修改。  
	2. 修改请求数据中的头信息，实现相应功能，比如模拟真实用户请求等功能。  
	3. 构建请求数据，随意进行数据提交。  
由此可以看出Fiddler断点功能分为两种类型：  
	1. 响应时断点  
	2. 请求时断点。  

