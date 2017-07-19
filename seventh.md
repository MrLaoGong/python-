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
设置响应断点的方法有两种，一种是通过可视化操作设置，另一种是通过命令去设置。  
如果要通过可视化操作设置，可以单击Fiddler中的Rules->Automatic Breakpoints->After Response。这只好之后，我们就可以对服务器的响应信息进行截取了。  
要取消对应的响应中断设置，可以再Fiddler中单击“Rules->Automatic Breakpoints->Disabled".  
除了使用可视化的方法设置响应中断外，还可以通过bpuafter命令设置，比如如果要中断csdn的响应信息，可以直接输入，bpuafter blog.csdn.net  
命令取消，bpuafter  
简单的应用实例  
首先设置响应中断，访问edu.51.com。收到响应后，将标签切换到TextView，下面的TextView。如果看到乱码，可以点击Responsebody is encoded.Click to decode.信息就会变成正常的。此时可以在编辑框内编辑你想更改的内容，然后点击，Run to Completion,此时会将对应的响应信息返回给火狐浏览器。  
请求中断  
第一种方法，可以通过Fiddler中的"Rules->Automatic Breakpoints->Before Requests",所有请求都会中断  
取消中断使用Disabled  
第二种方法，设置请求中断：  
bpu 网址a  
这种请求中断，只会对设置的网址起作用，对其他网址没有作用。  
取消中断使用 bpu  
###Fiddler会话查找功能  
通过Ctrl+F键调出Fiddler的会话查找界面，在窗口中输入查找信息，单击“Find Sessions"  
###Fiddler其他功能  
过滤指定域名的会话信息标记显示。  
首先，切换到Filters标签。  
勾选，“Use Filters",并在文本框中输入对应要过滤的域名。如果多个域名就用逗号隔开，如"iqiyi.com;blog.net;www.iqiyi.com"  
设置Hosts中第二个选项，  
* No Host Filter:不设置域名过滤  
* Hide the following Hosts:设置的这些会在会话中隐藏掉  
* Show only the following Hosts:在会话中只显示这些域名相关的会话
* Flag the following Hosts: 设置的这些域名会在会话列表中标记出来  
设置好之后，单击“Actions->Run Filterset now"

