#正则表达式与Cookie的使用
###正则表达式基础知识
1. 原子  
原子是正则表达式中最基本的组成单位，每个正则表达式中至少要包含一个原子，常见的原子有以下几类：  
	* 普通字符作为原子     
	* 非打印字符作为原子  
	* 通用字符作为原子  
	* 院子表  
    1. 普通字符作原子的情况。普通字符比如数字、大小写字母、下划线等都可以作为原子使用。比如如下程序中，我们就使用了“yue"作为原子使用，分别是'y''u''e'。
```
import re
pattern="yue"
string="http://yum.iqianyue.com"  
result1=re.search(pattern,string)
print(result1)
```  
  2. 非打印字符作为原子  
  	 所谓的非打印字符，指的是一些在字符串中用于格式控制的符号，比如换行符等。
```
	import re
	pattern="\n"
    string="http://yum.iqianyue.com
http://baidu.com"
result1=re.search(pattern.string)
print(result1)
```
	3. 通用字符作为原子   
所谓通用字符，即一个原子可以匹配一类字符，我们在实际项目中经常会用到这一类原子。    
| 符号 | 含义|
| \w | 匹配任意一个字母、数字或下划线 |  
| \W | 匹配除了字母、数字和下划线以外的任意字符|  
| \d | 匹配任意一个十进制数|  
| \D | 匹配除了十进制数以外的任意一个其他字符 |  
| \s | 匹配任意一个空白字符  |  
| \S | 匹配除空白字符以外的任意一个其他字符 |    	 
```
import re
pattern="\w\dpython\w"
string="abcdfphp345python_py"
result1=re.search(pattern,string)
print(result1)
```
	4. 原子表   
定义一组地位平等的原子，然后匹配的时候会取该原子表中的任意一个原子匹配，在Python中，原子表有[]表示比如[xyz]就是一个原子表，这个原子表中定义了3个原子，这3个原子的地位平等，如，我们定义的正则表达式为“[xyz]py”,对应的原字符串是"xpython",如果此时使用re.search()函数进行匹配，就可以匹配出结果"xpy",因为此时只要py前一位是x y z字母中的其中一个字母，就可以匹配成功。  
类似的，[^]代表的是除了中括号里面的原子均可以匹配，比如"[^xyz]py"能匹配"apy",但是却不能匹配"xpy"等。  
```
import re 
pattern1="\w\dpython[xyz]\w"
pattern2="\w\dpython[^xyz]\w"
pattern3="\w\dpython[xyz]\W"
string="abcdfphp345pythony_py"
result1=re.search(pattern1,string)
result2=re.search(pattern2,string)
result3=re.search(pattern3,string)
print(result1)
print(result2)
print(result3)
```  
2. 元字符  
所谓元字符，就是正则表达式中具有一些特殊含义的字符，比如重复N次前面的字符等。 
| 符号 | 含义 |  
| . | 匹配除换行符以外的任意字符 |  
| ^ | 匹配字符串的开始位置 |  
| $ | 匹配字符串的结束位置 |  
| * | 匹配0次、1次或多次前面的原子 |  
| ? | 匹配0次或1次前面的院子 |  
| + | 匹配1次或多次前面的原子 |  
| {n} | 前面的院子恰好出现n次 |  
| {n,} | 前面的原子至少出现n次 |  
| {n,m} | 前面的原子至少出现n次，至多出现m次 |  
| '|' | 模式选择符 |  
| () | 模式单元符 |  
具体来说元字符可以分为：任意匹配元字符、边界限制元字符、限定符、模式选择符、模式单元等。 
<<<<<<< HEAD
	* 任意匹配元字符  
	首先讲解任意匹配元字符 “.”,我们可以使用"."匹配一个除换行符以外的任意字符。比如，我们可以使用正则表达式".python..."匹配一个"python"字符前1位，后面有3位的格式的字符，这里的前一位和后面的3位可以是除了换行符以外的任意字符。
```
import re  
pattern=".python..."
string="abcdfphp345pythony_py"
result1=re.search(pattern,string)
print(result1)
```
	* 边界限制元字符  
	可以使用"^"匹配字符串的开始，使用"$"匹配字符串的结束
```
import re  
pattern1="^abd"  
pattern2="^abc"  
pattern3="py$"  
pattern4="ay$"  
string="abcdfphp345pythony_py"
result1=re.search(pattern1,string)  
result2=re.search(pattern2,string)  
result3=re.search(pattern3,string)  
result4=re.search(pattern4,string)  
print(result1)  
print(result2)  
print(result3)  
print(result4)  
```  
	* 限定符  
	常见的限定符包括*、?、+、{n}、{n,}、{n,m}.  
```
import re  
pattern1="py.*n"  
pattern2="cd{2}"  
pattern3="cd{3}"  
pattern4="cd{2,}"  
string="abcdddfphp345pythony_py"  
result1=re.search(pattern1,string)  
result2=re.search(pattern2,string)  
result3=re.search(pattern3,string)  
result4=re.search(pattern4,string)  
print(result1)  
print(result2)  
print(result3)  
print(result4)  
```  
	* 模式选择符  
	模式选择符“|”，可以设置多个模式，匹配时，可以从中选择任意一个模式匹配。比如正则表达式“python|php”中，字符串“python”和"php"均满足匹配条件。
```
import re  
pattern="python|php"  
string="abcdfphp345pythony_py"  
result1=re.search(pattern,string)  
print(result1)  
```
	* 模式单元符  
	模式单元符"()",可以将一些原子组合成一个大原子，小括号括起来的部分作为一个整体去使用。 
```
import re  
pattern1="(cd){1,}"  
pattern2="cd{1,}"  
string="abcdcdcdcdfphp345pythony_py"  
result1=re.search(pattern1,string)  
result2=re.search(pattern2,string)  
print(result1)  
print(result2)  
```
3. 模式修正  
所谓模式修正符，即可以在不改变正则表达式的情况下，通过模式修正符改变正则表达式的含义，从而实现一些匹配结果的调整等功能。比如，可以使用模式修正符I让对于模块在匹配时不区分大小写。  
| 符号 | 含义 |
| I | 匹配时忽略大小写 |  
| M | 多行匹配 |  
| L | 做本地化识别匹配 |  
| U | 根据Unicode字符及解析字符 |  
| S | 让.匹配包括换行符，即用了该模块修正后，"."匹配就可以匹配任意的字符了|  
```
import re  
pattern1="python"  
pattern2="python"  
string="abcdfphp34Pythony_py"  
result1=re.search(pattern1,string)  
result2=re.search(pattern2,string,re.I)  
print(result1)  
print(result2)  
```  
4. 贪婪模式与懒惰模式  
贪婪模式的核心点就是尽可能多地匹配，而懒惰模式的核心点就是尽可能少的匹配。  
```
import re  
pattern1="p.*y"#贪婪模式  
pattern2="p.*?y"#懒惰模式  
string="abcdfphp345pythony_py"  
result1=re.search(pattern1,string)  
result2=re.search(pattern2,string)  
print(result1)  
print(result2)  
```  
###正则表达式常见函数  
1. re.match()函数  
 如果想要从起始位置匹配一个模式，可以使用re.match（）函数，格式  
```
re.match(pattern,string,flag)
```  
第一个参数为正则表达式，第二个参数源字符串，第三个是可选参数，代表标志位。  
```
import re  
string="apythonhellomypythonhispythonourpythonend"  
pattern=".python."
result=re.match(pattern,string)  
result2=re.match(pattern,string).span()  
print(result)  
print(result2)  
```
2. re.search()函数  
re.search()函数会在全文中进行检索并匹配.  
```
import re  
string="hellomypythonhispythonourpythonend"  
pattern=".python."  
result=re.match(pattern,string)  
result2=re.search(pattern,string)  
print(result)  
print(result2)  
```  
3. 全局匹配函数  
匹配一串字符串中多个结果。  
	* 使用re.compile()对正则表达式进行预编译.  
	* 编译后，使用findall()根据正则表达式从源字符串中将匹配的结果全部找出。  、
```
import re  
string="hellomypythonhispythonourpythonend"  
pattern=re.compile(".python.")#预编译  
result=pattern.findall(string)#找出符合模式的所有结果  
print(result)  
```  
```
import re  
string="hellomypythonhistpythonourpythonend"  
pattern=".python."  
result=re.compile(pattern).findall(string)  
print(result)  
```  
4. re.sub函数  
使用正则表达式替换某些字符串功能，可以使用re.sub()函数实现。  
```
re.sub(pattern,rep,string,max)  
```  
第一个参数为正则表达式，第二个参数为要替换成的字符串，第三个参数为源字符串，第四个参数为可选项，代表最多替换的次数，不写的话，就全部替换。  
```
import re  
string="hellomypytonhispythonourpythonend"  
pattern="python."  
result1=re.sub(pattern,"php",string)#全部替换  
result2=re.sub(pattern,"php",string,2)#最多替换两次
print(result1)  
print(result2)  
```  
###常见实例解析 
* 匹配.com或.cn后缀的URL网址  
```
import re  
pattern="[a-zA-Z]+://[^\s]*[.com|.cn]"  
string="<a href='http://www.baidu.com'>百度首页</a>"
result=re.search(pattern,string)  
print(result)  
```  
* 匹配电话号码  
```
import re  
pattern="\d{4}-\d{7}|\d{3}-\d{8}"匹配电话号码的正则表达式  
string="0216728263682382265236"  
result=re.search(pattern,string)  
print(result)  
```  
* 匹配电子邮件地址  
```
import re  
pattern="\w+([.+-]\w+)*@\w+([.-]\w+)*\.\w+([.-]\w+)*"#匹配电子邮箱的正则表达式  
string="<a href='http://www.baidu.com'>百度首页</a><br><a href='mailto:c-e+o@iqianyue.com.cn'>电子邮箱地址</a>"
result=re.search(pattern,string)  
print(result)  
```  
###什么是Cookie    
Http是一个无状态协议，即无法维持会话之间的状态。Cookie可以保存状态。  
###Cookiejar实战  
```
import urllib.request  
import urllib.parse  
import http.cookiejar  
url="post请求地址"  
postdata=urllib.parse.urlencode({
	"username":"用户名"
	"password":"密码"
}).encode("utf-8")  
req=urllib.request.Request(url,postdata)  
req.add_header('User-Agent','User-Agent的值')  
cjar=http.cookieJar.CookieJar() #使用http.cookiejar.CookieJar()创建CookieJar对象 
opener=urllib.request.build_opener(urllib.request.HTTPCookieProcessor(cjar))#使用HTTPCookieProcessor创建cookie处理器,并以其为参数构建opener对象    
urllib.request.install_opener(opener)  #将opener安装为全局  
file=opener.open(req)  
data=file.read()  
file=open("文件存储地址","wb")  
file.write(data)  
file.close()  
url2="网页地址"  
data2=urllib.request.urlopen(url2).read()  
fh=open("文件地址",'wb')  
fh.write(data2)  
fh.close()  
```  

=======
>>>>>>> 3f577ee4216aa3d991983f31467a7d7578dd9797
