#python网络爬虫的学习
###什么是网络爬虫   
按照特定需求，从互联网中搜索有用信息网页进行过滤，就叫网络爬虫。   
###网络爬虫算法
当浏览信息的时候需要按照我们制定的规则进行，这些规则就叫做网络爬虫算法
###网络爬虫的组成
![zucheng](C://Users//My//Desktop//git学习//爬虫组成.png)   
网络爬虫由控制节点、爬虫节点、资源库构成。  
网络爬虫可以有多个控制节点,每个节点下有多个爬虫节点，控制节点间可以通信，控制节点和各爬虫节点也可以相互通信，属于同一个控制节点的爬虫节点也可以相互通信。  
控制节点也称为中央控制器，主要负责根据URL地址分配线程，并调用爬虫节点进行具体的爬行。  
爬虫节点会按照相关算法，对网页进行具体的爬行，主要包括下载网页，对网页文本处理，爬行后，会将对应的爬行结果存储在对应的资源库中。
###网络爬虫的类型
1. 通用网络爬虫  
又名全网爬虫，爬行范围非常大，数据海量，其爬取的性能要求非常高，主要应用于大型搜索引擎，有非常高的应用价值  
基本构成：  
	* 初始URL集合
	* URL队列
	* 页面爬行模块
	* 页面分析模块
	* 页面数据库
	* 链接过滤模块  
爬行策略：
	* 深度优先策略
	* 广度优先策略
2. 聚焦网络爬虫  
也叫主题爬虫，按照预先设定好的主题有选择的进行页面爬去的一种爬虫，目标网页定位与主题相关的页面中，范围比通用网络爬虫小，大大节省了爬虫爬取时所需的带宽资源和服务器资源。聚焦网络爬虫主要应用在特定的信息的爬取中，为某一类特定的人群服务
基本构成：
	* 初始URL集合
	* URL队列
	* 页面爬行模块
	* 页面分析模块
	* 页面数据库
	* 链接过滤模块  
	* 内容评价模块
	* 链接评价模块
爬行策略：  
	* 基于内容评价的爬行策略
	* 基于链接评价的爬行策略
	* 基于增强学习的爬行策略
	* 基于语境图的爬行策略
3. 增量式网络爬虫  
增量式指的是增量式更新，在爬取页面时，只爬取内容发生变化的网页或者新产生的网页，为发生变化的内容不会爬取。增量式爬虫尽可能的保证页面时最新的。
4. 深层网络爬虫  
在互联网中，网页分为表层页面和深层页面。表层页面就是静态页面，直接可以爬取。而深层页面需要提交表单，才能获取表单后面的页面。就是深层页面  
爬虫的构成：  
	* URL列表
	* LVS列表（LVS指的是标签/数值集合，即填充表单的数据源）
	* 爬行控制器
	* 解析器
	* LVS控制器
	* 表单分析器
	* 表单处理器
	* 响应分析器  
表单的填写：
	* 第一种基于领域知识的表单填写，建立一个填写表单的关键词库，需要填写的时候，根据语义分析选择对应关键词填写
	* 基于网页结构分析的表单填写，一般在领域知识有限的情况下使用，根据页面结构进行分析，并自动的进行表单填写
###聚焦爬虫详解
![zucheng](C://Users//My//Desktop//git学习//聚焦爬虫运行的流程.png)  
将初始的URL集合传递给URL队列，页面爬行模块会从URL队列中读取第一批URL列表，然后根据这些URL地址从互联网中进行相应的页面爬取。爬取后，将爬取到的内容传到页面数据库中存储，同时，在爬行过程中，会爬取新的URL，此时，需要根据我们所定的主题使用链接过滤模块过滤掉无关链接，再将剩下来的URL链接根据主题使用链接评价模块和内容评价模块进行优先级的排序。完成后，将新的URL地址传递到URL队列中，供页面爬行的模块使用。另一方面，将页面爬取并存放到页面数据库后，需要根据主题使用页面分析模块对爬取到的页面进行页面分析处理，并根据处理结果建立索引数据库，用户检索对应信息时，可以从索引数据库中进行相应的检索，并得到对应的结果。