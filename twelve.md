#开启scrapy爬虫项目之旅
###认识Scrapy项目的目录结构  
使用Scrapy创建一个爬虫项目。  
首先会生成一个与爬虫项目名称同名的文件夹，比如此时我们爬虫项目的名称为weisuen，此时，会生成一个名为weisuen的文件夹，该文件夹下拥有一个同名子文件夹（项目核心目录）和一个scrapy.cfg文件。  
该同名子文件夹放置的是爬虫项目的核心代码，Scrapy.cfg文件主要是爬虫项目的配置文件。  
该项目中同名子文件夹weisuen下放置了爬虫项目的核心代码，包括一个spiders文件夹，以及__init__.py,items.py,pipelines.py,settings.py等Python文件。  
这里提到的weisuen/weisuen/__init__.py文件为项目的初始化文件，主要写的是一些项目的初始化信息。  
weisuen/weisuen/items.py文件为爬虫项目的数据容器文件，主要用来定义我们要获取的数据。  
weisuen/weisuen/pipelines.py文件为爬虫项目的管道文件，主要用来对items里面定义的数据进行进一步的加工与处理。  
weisuen/weisuen/settings.py文件为爬虫项目的设置文件，主要为爬虫项目的一些设置信息。  
spiders文件夹下放置的是爬虫项目的爬虫部分相关的文件。  
weisuen/weisuen/spiders/__init__.py文件为爬虫项目中爬虫部分的初始化文件，主要对spiders进行初始化。  
###用Scrapy进行爬虫项目管理  
创建Scrapy爬虫  
首先使用cmd切换到你想将爬虫放置的目录，使用scrapy startproject 项目名 进行创建一个爬虫项目  
通过cd进入爬虫项目目录  
使用scrapy startproject -h 调出帮助信息  
--logfile=FILE参数主要用来指定日志文件，FILE为指定的日志文件的路径地址  
使用命令scrapy startproject --logfile="../log.txt" myproject，然后就可以在对应目录下看到对应的日志文件log.txt  
--loglevel=LEVEL,LEVEL参数主要用来控制日志信息的等级，默认为DEBUG模式，对应的还有其他等级，CRITICSL发生最严重的错误，ERROR发生了必须立即处理的错误，WARNING出现一些警告信息，即存在潜在错误，INFO输出一些提示信息，DEBUG输出一些调试信息，常用于开发阶段  
scrapy startproject --loglevel=DEBUG myproject2  
--nolog可以控制不输出日志信息  
想要删除scrapy爬虫，直接删除文件夹来实现。  
###常用工具命令
Scrapy中有两种命令，一种全局命令，一种项目命令  
全局命令不需要依靠Scrapy项目就可以在全局中直接运行，而项目命令必须要在Scrapy项目中才可以运行。  
1. 全局命令  
可以在不进入项目目录的情况下使用scrapy -h命令，此时cmd下会出现全局命令  
* fetch命令  
fetch命令主要用来显示爬虫爬取的过程  
scrapy fetch http://www.baidu.com  
此时如果在Scrapy项目之外使用该命令，则会调用Scrapy默认的爬虫来进行网页的爬取。如果在Scrapy的某个项目目录内使用该命令，则会调用该项目中的爬虫来进行网页的爬取。  
使用scrapy fetch -h 查看帮助文档。   
--headers参数显示对应的爬虫爬取网页时候的头信息，--spider=SPIDER控制使用哪个爬虫。  
* runspider命令  
使用runspider命令我们可以实现不依托Scrapy的爬虫项目，直接运行一个爬虫文件。  
scrapy runspider first.py  
* settings命令  
使用settings命令查看Scrapy对应的配置信息。  
如果在scrapy项目外使用，查看的使用scrapy默认配置信息，如果在Scrapy项目目录内使用settings命令，查看的是项目的配置信息。  
进入到项目目录  
使用 scrpay settings --get BOT_NAME  可以发现和项目里的scrapy.cfy里BOT_NAME内容一样  
* shell命令  
通过shell命令可以启动Scrapy的交互终端（Scrapy shell）
命令：scrapy shell http://www.baidu.com --nolog  
出现终端交互界面，输入exit（）退出  
* startproject命令  
创建项目  
* version命令  
version命令显示Scrapy版本信息  
scrapy version  
scrapy version -v 查看详细scrapy版本信息  
* view命令  
通过view命令，我们可以实现下载某个网页并用浏览器查看的功能。  
scrapy view http://new.163.com/  
2. 项目命令  
项目命令进入scrapy项目里才生效  
scrapy -h  
* Bench命令  
使用bench命令可以测试本地硬件的性能  
scrapy bench  
* Genspider命令  
使用genspider命令创建scrapy爬虫文件，这是一种快速创建爬虫文件的方式  
该命令基于现有的爬虫模板直接生成一个新的爬虫文件。需要在scrapy爬虫项目中使用  
查看爬虫模板  scrapy genspider -l  
创建  scrapy genspider -t basic weisuen iqianyue.com  
使用-d查看模板内容  
scrapy genspider -d csvfeed  
* Check命令  
爬虫的测试比较麻烦。使用Scrapy中的check命令，可以对某个爬虫文件进行合同（contract）检查。  
scrapy check 爬虫名 是爬虫名不是文件名没有后缀  
scrapy check weisuen  
* Crawl命令  
可以通过crawl命令来启动某个爬虫，启动格式是“scrapy crawl 爬虫名”  
scrapy crawl weisuen --loglevel=INFO  
* List命令  
通过Scrapy中的list命令，可以列出当前可使用的爬虫文件  
scrapy list  
* Edit命令  
windows里使用会有问题  
* Parse命令  
通过parse命令，我们可以实现获取指定的URL网址，并使用对应的爬虫文件进行处理和分析。  
scrapy parse http://www.baidu.com  
帮助文档 scrapy parse -h  
帮助文档中可以看出，参数分两类：普通参数和全局参数  
parse命令对应的参数表  
--spider=SPIDER 强行指定某个爬虫文件spider进行处理  
-a NAME=VALUE 设置spider的参数，可能会重复  
--pipelines  通过pipelines来处理items  
--nolinks  不展示提取到的链接信息  
--nocolour 输出结果颜色不高亮  
--rules，-r  使用CrawlSpider规则去处理回调参数  
--callback=CALLBACK，-c CALLBACK指定spider中用于处理返回的响应的回调函数  
--depth=DEPTH，-d DEPTH设置爬行深度，默认深度为1  
--verbose，-v 显示每层的详细信息  


