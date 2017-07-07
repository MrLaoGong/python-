#Urllib库与URLError异常处理
###什么是Urllib库
Urllib是Python提供的一个用于操作URL的模块。
###快速使用Urllib爬取网页
首先导入模块  
```
import urllib.request
```  
导入后，爬取  
```
file = urllib.request.urlopen("http://www.baidu.com")
```  
读取，三种方式  
1. file.read()读取文件的全部内容，与readlines不同的是，read会把读取到的内容赋给一个字符串变量  
2. file.readlines()读取文件的全部内容，与read不同的是，readlines会把读取到的内容赋给一个列表变量，若要读取全部内容，推荐使用这种方式  
3. File.readline()读取文件的一行文件。  
如何将爬取到的网页保存到本地以网页形式  
1. 首先，爬取一个网页并将爬取到的内容赋值给一个变量。  
2. 以写入的方式打开一个本地文件，命名为*.html等网络格式。  
3. 将1中变量写入文件  
4. 关闭该文件。  
```
fhandle=open(r"C:\Users\My\Desktop\git学习\1.html",'wb')
fhandle.write(file.read())
fhandle.close()
```
###urlretreve()函数
直接将对应信息写入文件。格式：`urllib.request.urlretrieve(url,filename=本地文件地址)`

```
urllib.request.urlretrieve("http://www.51cto.com/",filename=r"C:\Users\My\Desktop\git学习\2.html")
```  
urlretrieve执行的过程中，会产生缓存，清除缓存  
```
urllib.request.urlcleanup()
```  
获取与当前环境相关的信息使用info。
```
file.info()
```   
获取当前爬取网页的状态码，使用getcode(),若返回200为正确，返回其他的不正确。  
```
file.getcode()
```    
获取爬取网页的URL，使用geturl()
```
file.geturl()
```    
一般来说,URL标准中只会允许一部分ASCII字符比如数字、字母、部分符号等，而其他的一些字符，比如汉字等，是不符合URL标准的。所以如果我们在URL中使用一些其他不符合标准的字符就会出现问题，此时需要进行URL编码方可解决。比如在URL中输入中文或者“：”或者“&”等不符合标准的字符时，需要编码。  
如果要进行编码，使用urllib.request.quete()进行,比如,我们对“http://www.sina.com.cn"进行编码。
```
urllib.reqeust.quote("http://www.sina.com.cn")
输出'http%3A//www.sina.com.cn'
```  
解码  
```
urllib.request.unquote("http%3A//www.sina.com.cn")
输出'http://www.sina.com.cn'
```  
###浏览器的模拟---Headers属性  
有时候，我们无法爬取一些网页，出现403错误，这是因为这些网页为了防止别人恶意采集信息进行的反爬虫的设置。  
如果想爬取，我们可以设置Headers信息，模拟成浏览器去访问这些网站。设置Headers，首先需要的是找出User-Agent。  
![User-Agent](C:\Users\My\Desktop\git学习)  
有两种方法让爬虫模拟成浏览器范文网页  
1. 使用build_opener()修改报头  
```
import urllib.request
url="网页地址"
headers=("User-Agent","浏览器里找到的User-Agent")
opener=urllib.request.build_opener()
opener.addheaders=[headers]
data=opener.open(url).read()
fhandler=open("存放网页的地址","wb")
fhandler.write(data)
fhandler.close()
```  
2. 使用add_header()添加报头  
```
import urllib.request
url="网页地址"
req=urllib.request.Request(url)
req.add_header('User-Agent',"浏览器找到的User-Agent")
data=urllib.request.urlopen(req).read()
```  
###超时设置  
有的时候，访问一个网页，如果该网页长时间未响应，那么系统判断该网页超时，即无法打开该网页。  
```
import urllib.request
file=urllib.request.urlopen("网页地址",timeout=时间值)
```
###HTTP协议请求实战  
HTTP协议请求主要分为6种类型。  
1. GET请求：GET请求会通过URL网址传递信息，可以直接在URL中写上要传递的信息，也可以有表单进行传递。如果使用表单进行传递，这表单中的信息会自动转为URL地址中的数据，通过URL地址传递。  
2. POST请求：可以向服务器提交数据，是一种比较主流也比较安全的数据传递方式，比如在登录时，经常使用POST请求发送数据。  
3. PUT请求：请求服务器存储一个资源，通常要指定存储的位置。  
4. DELETE请求：请求服务器删除一个资源  
5. HEAD请求：请求获取对应的HTTP报头信息。  
6. OPTIONS请求：可以获得当前URL所支持的请求类型。  
除此之外还有TRACE请求和CONNECT请求等，TRACE请求主要用于测试或诊断。  
详细讲解GET与POST  
- GET请求实例分析    
```
	import urllib.request  
	keywd="hello"  
	url="http://www.baidu.com/s?wd="+keywd  
	req=urllib.request.Request(url)  
	data=urllib.request.urlopen(req).read()  
	fhandle=open("文件存储地址","wb")  
	fhandle=write(data)
	fhandle.close()
```  
当检索中文会报错。  
`UnicodeEncodeError`  
```
import urllib.request
url="http://www.baidu.com/s?wd="
key="韦玮老师"
key_code=urllib.request.quote(key)
url_all=url+key_code
req=urllib.request.Request(url_all)
data=urllib.request.urlopen(req).read()
fh=open("文件地址","wb")
fh.write(data)
fh.close()
```    
使用GET请求，思路如下:  
	1. 构建对应的URL地址，该URL地址包含GET请求的字段名和字段内容等信息，并且URL地址满足GET请求的格式，即“http://网址?字段名1=字段内容1&字段名2=字段内容2”。  
	2. 对应的URL为参数，构建Request对象。  
	3. 通过urlopen（）打开构建的Request对象。  
	4. 按需求进行后续的处理操作。比如读取网页内容，或写入文件。  
- POST请求实例分析  
使用Post请求，思路如下：
	1. 设置好URL网址。  
	2. 构建表单数据，并使用urllib.parse.urlencode对数据进行编码处理  
	3. 创建Request对象，参数包括URL地址和要传递的数据。  
	4. 使用add_header()添加头信息，模拟浏览器进行爬取。  
	5. 使用urllib.request.urlopen()打开对应的Request对象，完成信息的传递。  
	6. 后续处理，比如读取，写入文件等。  
```
import urllib.request  
import urllib.parse  
url="网页地址"
postdata=urllib.parse.urlencode({
'name':'aaa',
'pass':'bbb'}).encode('utf-8')#将数据使用urlencode编码处理后，使用encode()设置为utf-8编码
req=urllib.request.Request(url,postdata)  
req.add_header('User-Agent','User-Agent数据')
data=urllib.request.urlopen(req).read()
fd=open("存储地址",'wb')
fd.write(data)
fd.close()
```  
###代理服务器的设置  

```
	#代理服务器网站http://yum.iqianyue.com/proxy
def use_proxy(proxy_addr,url):
	import urllib.request
	proxy=urllib.request.ProxyHandler({'http':proxy_addr})
	opener=urllib.request.build_opener(proxy,urllib.request.HTTPHandler)
	urllib.request.install_opener(opener)
	data=urllib.request.urlopen(url).read().decode('utf-8')
	return data
proxy_addr="代理服务器地址加端口"
data=use_proxy(proxy_addr,"网页地址")
print(len(data))
```  
###DebugLog实战
###异常处理神器----URLError实战
```
import urllib.request
import urllib.error
try:
	urllib.request.urlopen("网页地址")
except urllib.error.URLError as e:
	print(e.code)
	print(e.reason)
输出错误：
	403
	Forbidden
```  
产生URLError的原因有如下几种可能：
1. 链接不上服务器
2. 远程URL不存在
3. 无网络
4. 触发了HTTPError
403触发了是URLError的子类HTTPError，所以可以改成
```
import urllib.request
import urllib.error
try:
	urllib.request.urlopen("网页地址")
except urllib.error.HTTPError as e:
	print(e.code)
	print(e.reason)
```
状态码含义：
200 OK  
一切正常  
301 Moved Permanently  
重定向到新的URL，永久性  
302 Found  
重定向到临时的URL，非永久性  
304 Not Modified  
请求的资源为更新  
400 Bad Request  
非法请求  
401 Unauthorized  
请求未经授权  
403 Forbidden  
禁止访问  
404 Not Found  
没有找到对应页面  
500 Internal Server Error  
服务器内部出现错误  
501 Not Implemented  
服务器不支持实现请求所需要的功能  
HTTPError无法处理URLError的前三个错误。处理会报错。
```
import urllib.request
import urllib.error
try:
	urllib.request.urlopen("http://blog.baidusss.net")
except urllib.error.HTTPError as e:
	print(e.code)
	print(e.reason)
except urllib.error.URLError as e:
	print(e.reason)
```
