#手写python爬虫
###图片爬虫实战  
思路
1. 建立一个爬取图片的自定义函数，该函数负责爬取一个页面下的我们想爬取的图片，爬取过程为：首先通过urllib.request.urlopen(url).read()读取对应网页的全部源代码，然后根据上面的第一个正则表达式进行第一次信息过滤，过滤完成之后，在第一次过滤结果的基础上，根据上面的第二个正则表达式进行第二次信息过滤，提取出该网页上所有目标图片的链接，并将这些链接地址存储的一个列表中，随后遍历该列表，分别将对应链接通过urllib.request.urlretrieve(imageurl,filename=imagename)存储到本地，为了避免程序中途异常崩溃，我们可以建立异常处理，若不能爬取某个图片，则会通过x+=1自动跳到下一个图片。  
2. 通过for循环将该分类下的所有网页都爬取一遍，链接可以构造为url="http://list.jd.com/list.html?cat=23413143151&page="+str(i),在for循环里面，每一次循环，对应的i会自动加1，每次循环的时候通过调用1）中的函数实现该也图片的爬取  
```    
import re  
import urllib.request  
def craw(url,page):  
    html1=urllib.request.urlopen(url).read()  
    html1=str(html1)  
    pat1='<div id="plist".+?<div class="page clearfix">'  
    result1=re.compile(pat1).findall(html1)  
    result1=result1[0]  
    pat2='<img width="220" height="220" data-img="1" data-lazy-img="//(.+?\.jpg)">'  
    imagelist=re.compile(pat2).findall(result1)  
    # imagelist=re.search(pat2,result1)  
    x=1  
    for imageurl in imagelist:  
        imagename="D:/shoujitupian/img/"+str(page)+str(x)+".jpg"  
        imageurl="http://"+imageurl  
        try:  
            urllib.request.urlretrieve(imageurl,filename=imagename)  
        except urllib.error.URLError as e:  
            if hasattr(e,"code"):  
                x+=1  
            if hasattr(e,"reason"):  
                x+=1  
        x+=1  
  
for i in range(1,6):  
    url="https://list.jd.com/list.html?cat=9987,653,655&page="+str(i)  
    craw(url,i)  
   
```  
###链接爬虫  
1. 确定好要爬取的入口链接。
2. 根据需求构建好链接提取的正则表达式。
3. 模拟成浏览器并爬取对应网页。
4. 根据2中的正则表达式提取出该网页中包含的链接。
5. 过滤掉重复的链接。
6. 后续操作。比如打印这些链接到屏幕上等。  
```
__author__ = 'My'

import re#爬取所有页面链接  
import urllib.request  
def getlinks(url):  
    headers=('User-Agent','Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36') #模拟成浏览器  
    opener=urllib.request.build_opener() 
    opener.addheaders=[headers]  
    urllib.request.install_opener(opener)#将opener安装为全局  
    file=urllib.request.urlopen(url)  
    data=str(file.read())  
    pat='(https?://[^\s)";]+\.(w|/)*)'#根据需求构建好链接表达式  
    link=re.compile(pat).findall(data)  
    link=list(set(link))#去除重复元素  
    return link  
url="http://blog.csdn.net/"#要爬取的网页链接   
linklist=getlinks(url)#获取对应网页中包含的链接地址  
for link in linklist:#通过for循环分别遍历输出获取到的链接地址到屏幕上  
    print(link[0])  
```  
###糗事百科爬虫实战  
1. 分析各页间的网址规律，构造网址变量，并可以通过for循环实现多页内容的抓取  
2. 构建一个自定义函数，专门用来实现抓取某个网页上的段子，包括两部分内容，一部分是对应用户，一部分是用户发表的段子内容。该函数功能实现的过程为：首先，模拟成浏览器访问，观察对应网页源代码中的内容，将用户信息部分与段子内容部分的格式写成正则表达式。随后，根据正则表达式分别提取出改业中所有用户与所有内容，然后通过for循环遍历段子内容并将内容分别赋给对应的变量，这里变量名是有规律的，格式为“content+顺序号",接下来在通过for循环遍历对应用户，并输出该用户对应的内容。  
3. 通过for循环分别获取多页的各页URL链接，每页分别调用一次getcontent（url，page）函数。  
```   
__author__ = 'My'  
import  re  
import urllib.request  
def getcontent(url,page):  
    headers=('User-Agent','Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36')     #模拟成浏览器  
    opener=urllib.request.build_opener()  
    opener.addheaders=[headers]  
    urllib.request.install_opener(opener)  #将opener安装为全局  
    file=urllib.request.urlopen(url)  
    data=str(file.read().decode("utf-8"))  
    userpat='target="_blank" title="(.*?)">'#提取用户的正则表达式  
    contentpat='<div class="content">(.*?)</div>'#提取内容的正则表达式  
    userlist=re.compile(userpat,re.S).findall(data)  
    contentlist=re.compile(contentpat,re.S).findall(data)  
    x=1  
    for content in contentlist:  
        content=content.replace("\n","")  
        content=content.replace("<span>","")  
        content=content.replace("</span>","")  
        content=content.replace("<br/>","")  
        name="content"+str(x)  
        exec(name+'=content')  
        x+=1  
    y=1  
    for user in userlist:  
        name="content"+str(y)  
        print("用户"+str(page)+str(y)+"是:"+user)  
        print("内容是:")  
        exec("print("+name+")")  
        print("\n")  
        y+=1  
for i in range(1,10):  
    url="https://www.qiushibaike.com/8hr/page/"+str(i)  
    getcontent(url,i)  
```  
###微信爬虫实现  
1. 建立3个自定义函数：一个函数实现使用代理服务器爬取指定网址并返回爬取到的数据的功能，一个函数实现获取多个页面的所有文章链接的功能，另外一个函数实现根据文章链接爬取指定标题和内容并写入文件的功能。
2. 使用代理服务器爬取指定网址的内容的功能在4章提过，为避免一场导致程序中断，所以建立异常处理机制
3. 要实现获取多个页面的所有文章链接，我们需要对关键词使用urllib.request.quote(key)进行编码，编码后构造出对应的文章列表页网址，并通过for循环依次爬取各页的文章链接，爬取时，通过调用2中设置的代理服务器实现。
4. 要实现根据文章链接爬取指定标题和内容并写入对应文件中，可以使用for循环依次爬取3中所提供的网址（真实网址),爬取后根据正则表达式提取出我们关注的内容并写入对应的文件中。  
5. 代码中如果发生异常，需要进行延时处理，即等待一段时间之后再尝试下一次操作。要实现延时处理，我们可以导入time模块，使用time.sleep（）实现，比如time.sleep(7)延时7秒。
```
__author__ = 'My'  
import re  
import urllib.request  
import time   
import urllib.error  
#模拟浏览器  
headers=('User-Agent','Mozilla/5.0 (Windows NT 6.1; WOW64)   AppleWebKit/537.36 (KHTML, like Gecko)     Chrome/45.0.2454.101 Safari/537.36')  
opener=urllib.request.build_opener()  
opener.addheaders=[headers]  
#將opener安裝為全局  
urllib.request.install_opener(opener)  
#設置一个列表listurl存储文章网页列表  
listurl=[]  
#设置代理ip  
def use_proxy(proxy_addr,url):  
    #建立异常机制  
    try:  
        import urllib.request  
        proxy=urllib.request.ProxyHandler   ({'http':proxy_addr})  
        opener=urllib.request.build_opener    (proxy,urllib.request.HTTPHandler)  
        urllib.request.install_opener(opener)  
        data=urllib.request.urlopen(url).read().decode('utf-8')  
        return data  
    except urllib.error.URLError as e:  
        if hasattr(e,'code'):  
            print(e.code)  
        if hasattr(e,'reason'):  
            print(e.reason)  
        time.sleep(10)  
    except Exception as e:  
        print('exception:'+str(e))  
        time.sleep(1)  
  
#设置搜索  
def getlisturl(key,pagestart,pageend,proxy):  
    try:  
        page=pagestart  
        #编码关键词key  
        keycode=urllib.request.quote(key)  
        #编码"&page"  
        pagecode=urllib.request.quote('&page')  
        #循环爬取各页文章的链接  
        for page in range(pagestart,pageend+1):  
            #分别构建各页的url链接，每次循环构建一次  
            url="http://weixin.sogou.com/weixin?type=2&query="+keycode+pagecode+str(page)  
            '''  
            http://weixin.sogou.com/weixin?query=物联网  &type=2&page=2  
            '''  
            #用代理服务器爬取，解决ip封杀问题  
            data1=use_proxy(proxy,url)  
            print(data1)  
            #获取链接正则表达式  
            listurlpat='<div class="txt-box">.*?  (http://.*?)"'   
            #获取每页的所有文章链接并添加到列表listurl中  
            print(re.compile(listurlpat,re.S).findall  (data1))  
            listurl.append(re.compile (listurlpat,re.S).findall(data1))  
        print("获取到",str(len(listurl)),'页')  
        return listurl  
    except urllib.error.URLError as e:  
        if hasattr(e,'code'):  
            print(e.code)  
        if hasattr(e,'reason'):  
            print(e.reason)  
        time.sleep(10)  
    except Exception as e:  
        print('exception:'+str(e))  
        time.sleep(1)  
#设置保存网页     
def getcontent(listurl,proxy):  
    i=0  
    #html头  
    html1='''  
    <html><head>  
    <title>微信文章页面</title>  
    </head>  
    <body>  
        '''  
    fh=open("D:/111.html","wb")  
    fh.write(html1.encode('utf-8'))  
    fh.close()  
    #再次追加写入的方式打开文件，以写入对应文章内容  
    fh=open('D:/111.html','ab')  
    #此时listurl  
    print(listurl)  
    for i in range(0,listurl):  
        for j in range(0,listurl[i]):  
            try:  
                url=listurl[i][j]  
                url=url.replace('amp;',"")  
                data=use_proxy(proxy,url)  
                titlepat="<title>(.*?)</title>"  
                contentpat='id="js_content">(.*?)  id="js_sg_bar"'  
                title=re.compile(titlepat).findall(data)  
                content=re.compile(contentpat).findall (data)  
                thistitle="此次没有获取到"  
                thiscontent="此次没有获取到"  
                if(title!=[]):  
                    thistitle=title[0]  
                if(content!=[]):  
                    thiscontent=content[0]  
                dataall="<p>标题为:"+thistitle+"</p><p>内容为："+thiscontent+"</p><br>"  
                fh.write(dataall.encode('utf-8'))  
                print("第"+str(i)+"个网页第"+str(j)+" 次处理")  
            except urllib.error.URLError as e:  
                if hasattr(e,'code'):  
                    print(e.code)  
                if hasattr(e,'reason'):  
                    print(e.reason)  
                time.sleep(10)  
            except Exception as e:  
                print('exception:'+str(e))  
                time.sleep(1)  
    fh.close()  
    html2='''  
    </body>  
    </html>  
        '''  
    fh.open("D:/111.html",'ab')  
    fh.write(html2.encode('utf-8'))  
    fh.close()  
key='物联网'  
proxy="125.115.183.26	:808"  
proxy2=""  
pagestart=1  
pageend=2  
listurl=getlisturl(key,pagestart,pageend,proxy)  
getcontent(listurl,proxy)  
```  
###多线程爬虫
多线程小程序  
```
import threading  
class A(threading.Thread):  
    def __init__(self):  
        threading.Thread.__init__(self)  
    def run(self):  
        for i in range(10):  
            print('我是线程A')  
class B(threading.Thread):  
    def __init__(self):  
        threading.Thread.__init__(self)  
    def run(self):  
        for i in range(10):  
            print('我是线程B')  

t1=A()  
t2=B()  
t1.start()   
t2.start()  
```  
队列的使用  
```
import queue  
a=queue.Queue()  
a.put('hello')  
a.put('wj')  
a.put('like')  
a.put('study')  
a.task_done()  
print(a.qsize())  
print(a.get())  
print(a.get())  
print(a.get())  
print(a.get())  
print(a.qsize())  
print(a.get(),'----')  
```   
微信小程序改成多线程提升效率  
```
# -*- coding: utf-8 -*-  
"""  
Created on Sat Apr 22 10:25:08 2017  
  
@author: My  
"""  
  
import threading  
import queue  
import re  
import urllib.request  
import time  
import urllib.error  
urlqueue=queue.Queue()  
headers=('User-Agent','Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36')  
opener=urllib.request.build_opener()  
opener.addheaders=[headers]  
urllib.request.install_opener(opener)  
listurl=[]  
def use_proxy(proxy_addr,url):   
    try:  
        import urllib.request  
        proxy=urllib.request.ProxyHandler  ({'http://':proxy_addr})  
        opener=urllib.request.build_opener(proxy,urllib.request.HTTPHandler)  
        urllib.request.install_opener(opener)  
        print(url)  
        data=urllib.request.urlopen(url).read().decode('utf-8')  
        return data  
    except urllib.error.URLError as e:  
        if hasattr(e,"code"):  
            print(e.code)  
        if hasattr(e,"reason"):  
            print(e.reason)  
        time.sleep(10)  
    except Exception as e:  
        print("exception:"+str(e))  
        time.sleep(1)  
class geturl(threading.Thread):  
    def __init__(self,key,pagestart,pageend,proxy,urlqueue):  
        threading.Thread.__init__(self)  
        self.pagestart=pagestart   
        self.pageend=pageend  
        self.proxy=proxy  
        self.urlqueue=urlqueue  
        self.key=key  
    def run(self):  
        page=self.pagestart  
        keycode=urllib.request.quote(key)  
        pagecode=urllib.request.quote("&page")  
        for page in range(self.pagestart,self.pageend+1):  
            url="http://weixin.sogou.com/weixin? type=2&query="+keycode+pagecode+str(page)  
            print(url)  
            data1=use_proxy(self.proxy,url)  
            listurlpat='<div class="txt-box">.*? (http://.*?)"'  
            listurl.append(re.compile (listurlpat,re.S).findall(data1))  
            print("获取到："+str(len(listurl))+"页")  
            for i in range(0,len(listurl)):  
                time.sleep(7)  
                for j in range(0,len(listurl[i])):  
                    try:  
                        url=listurl[i][j]  
                        url=url.replace("amp;","")  
                        print("第"+str(i)+"i"+str(j)+"j次入队")  
                        self.urlqueue.put(url) 
                        self.urlqueue.task_done()  
                    except urllib.error.URLError as e:  
                        if hasattr(e,"code"):  
                            print(e.code)  
                        if hasattr(e,"reason"):  
                            print(e.reason)  
                        time.sleep(10)  
                    except Exception as e:  
                        print("exception:"+str(e))  
                        time.sleep(1)  
class getcontent(threading.Thread):  
    def __init__(self,urlqueue,proxy):  
        threading.Thread.__init__(self)  
        self.urlqueue=urlqueue  
        self.proxy=proxy  
    def run(self):  
        html1='''<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR、xhtml1/DTD/xhtml1-transitional.dtd">
        <html xmlns="http://www.w3.org/1999/xhtml">  
        <head> 
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>  
        <title>微信文章页面</title>  
        </head>  
        <body>'''  
        fh=open("D:/pythontest/7.html",'wb')  
        fh.write(html1.encode('utf-8'))  
        fh.close()  
        fh=open("D:/pythontest/7.html",'ab')  
        i=1    
        while(True):  
            try:  
                url=self.urlqueue.get()  
                data=use_proxy(self.proxy,url)  
                titlepat="<title>(.*?)</title>"  
                contentpat='id="js_content">(.*?)  id="js_sg_bar"'  
                title=re.compile(titlepat).findall(data)  
                content=re.compile  (contentpat,re.S).findall(data)  
                thistitle="此次没有获取到"  
                thiscontent="此次没有获取到"  
                if(title!=[]):  
                    thistitle=title[0]  
                if(content!=[]):  
                    thiscontent=content[0]  
                    dataall="<p>标题为："+thistitle+"</p><p>内容为："+thiscontent+"</p></br>"
                    fh.write(dataall.encode("utf-8"))
                    print("第"+str(i)+"个网页处理")  
                    i+=1  
            except urllib.error.URLError as e:  
                if hasattr(e,'code'):  
                    print(e.code)  
                if hasattr(e,"reason"):  
                    print(e.reason)  
                time.sleep(10)  
            except Exception as e:  
                print("exception:"+str(e))  
                time.sleep(1)  
                fh.close()  
                html2='''</body>  
                </html>  
                '''  
                fh=open("D:/pythontest/7.html",'ab')  
                fh.write(html2.encode('utf-8'))  
                fh.close()  
class conrl(threading.Thread):  
    def __init__(self,urlqueue):  
        threading.Thread.__init__(self)  
        self.urlqueue=urlqueue  
    def run(self):  
        while(True):  
            print("程序执行中")  
            time.sleep(60)  
            if(self.urlqueue.empty()):  
                print("程序执行完毕！")  
                exit()  
key="物联网"  
proxy="59.61.92.205:8118"  
proxy2=""  
pagestart=1  
pageend=2  
t1=geturl(key,pagestart,pageend,proxy,urlqueue)  
t1.start()  
t2=getcontent(urlqueue,proxy)  
t2.start()  
t3=conrl(urlqueue)  
t3.start()          
```  
