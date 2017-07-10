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