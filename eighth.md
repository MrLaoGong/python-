#第八章 爬虫的浏览器伪装技术  
### 什么是浏览器伪装技术  
有一些网站为了避免爬虫的恶意访问，会设置一些反爬虫机制，常见的反爬虫机制主要有：  
1. 通过分析用户请求的Headers信息进行反爬虫  
2. 通过检测用户行为进行反爬虫，比如通过判断同一个IP在短时间内是否频繁访问对应网站等进行分析。  
3. 通过动态页面增加爬虫爬取难度，达到反爬虫的目的。  
第一种反爬虫机制在目前的网站中应用得最多。  
第二种可以通过经常切换静态代理服务器的方式，一般就可以。  
第三种可以使用一些工具软件，比如selenium+phantomJS。  
###浏览器伪装技术准备工作  
常见头信息中字段含义。  
常见字段1：Accept:text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8  
* Accept字段主要用来表示浏览器能够支持的内容类型有哪些。  
* text/html表示HTML文档  
* application/xhtml+xml表示XHTML文档。  
* application/xml 表示XML文档。  
* q代表权重系数，值介于0和1之间。  
所以这一行字段信息表示浏览器可以支持text/html、application/xhtml+xml、application/xml、*/*等内容，支持的优先顺序从左到右依次排列。  
常见字段2： Accept-Encoding:gzip,deflate  
* Accept-Encoding字段主要用来表示浏览器支持的压缩编码有哪些。  
* gzip是压缩编码的一种。  
* deflate是一种无损数据压缩算法。  
这一行字段信息表示浏览器可以支持gzip、defalte等压缩编码。  
常见字段3：Accept-Language:zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3  
* Accept-Language主要用来表示浏览器所支持的语言类型。
* zh-CN表示简体中文语言，zh表示中文，CN表示简体  
* en-US表示英语（美国)语言  
* en表示英语语言  
所以这一行字段表示浏览器可以支持zh-CN，zh、en-US、en等语言。  
常见字段4：User-Agent:Mozilla/5.0(WindowsNT6.1;WOW64;rv:47.0)Gecko/20100101 Firefox/47.0  
* User-Agent字段主要表示用户代表，服务器可以通过该字段识别出客户端的浏览器类型、浏览器版本、客户端的操作系统及版本号，网页排版引擎等客户端信息。所以我们之前要模拟浏览器登录，主要以伪造该字段进行。   
* Mozilla/5.0表示浏览器名及版本信息。  
* Windows NT 6.1;WOW64;rv:47.0表示客户端操作系统对应信息。  
* Gecko表示网页排版引擎对应信息  
* firefox自然表示火狐浏览器  
所以这一行字段表示的信息为对应的用户代理信息是Mozilla/5.0（Windows NT 6.1;WOW64;rv:47.0)Gecko/20100101 Firefox/47.0   
常见字段5：Connection:keep-alive  
* Connection表示客户端与服务器的连接类型，对应的字段值主要有两种  
	1. keep-alive表示持久性链接
	2. close表示单方面关闭连接，让连接断开  
所以此时，表示客户端与服务器端的链接是持久性的  
常见字段6： Host：www.youku.com  
* Host字段表示请求的服务器网址是什么，此时表示请求优酷网址  
常见字段7：Referer：网址  
* Refere字段主要表示原网址地址，比如我们从http://www.youku.com网址中访问了该网址下的子页面http://tv.youku.com/?spm=0.0....QnQOEf,那么此时来原网址为：http://www.youku.com,即此时Referer字段的值为http://www.youku.com  
###爬虫的浏览器伪装技术实战  

