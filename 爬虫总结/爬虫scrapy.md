### 一、Scrapy框架介绍

> **Scrapy，Python开发的一个快速,高层次的屏幕抓取和web抓取框架，用于抓取web站点并从页面中提取结构化的数据。Scrapy用途广泛，可以用于数据挖掘、监测和自动化测试。 Scrapy吸引人的地方在于它是一个框架，任何人都可以根据需求方便的修改。**

### 二、Scrapy框架的简单使用

>创建项目：scrapy startproject xxx
>
>进入项目：cd xxx 进入某个文件夹下
>
>创建爬虫：scrapy genspider xxx（爬虫名） xxx.com （爬取域）
>
>生成文件：scrapy crawl xxx -o xxx.json (生成某种类型的文件)
>
>运行爬虫：scrapy crawl XXX
>
>列出所有爬虫：scrapy list

* 创建一个scrapy项目,查看一下里面存在的详细文件

> scrapy startproject search
>
> scrapy.cfg: 项目的配置文件
>
> search/: 该项目的python模块。在此放入代码（核心）
>
> search/items.py: 项目中的item文件.（这是创建容器的地方，爬取的信息分别放到不同容器里）
>
> search/pipelines.py: 项目中的pipelines文件.
>
> search/settings.py: 项目的设置文件.（我用到的设置一下基础参数，比如加个文件头，设置一个编码）

* <font color=red>容器</font>（items）的定义，容器不一定是一开始全部都定义好的，可以跟随项目的更新一点点向里面添加

> import scrapy
>
> class DmozItem(scrapy.Item): #创建一个类，继承scrapy.item类，就是继承人家写好的容器
>
> title = scrapy.Field() # 需要取哪些内容，就创建哪些容器
>
> link = scrapy.Field()

* 一个简单的爬虫小例子

```
import scrapy
class DmozSpider(scrapy.Spider): # 继承Spider类
    name = "dmoz" # 爬虫的唯一标识，不能重复，启动爬虫的时候要用
    allowed_domains = ["dmoz.org"] # 限定域名，只爬取该域名下的网页
    start_urls = [ # 开始爬取的链接
        "https://www.baidu.com/"
    ]
    def parse(self, response):
        filename = response.url.split("/")[-2] # 获取url，用”/”分段，获去倒数第二个字段
        with open(filename, 'a') as f:
            f.write(response.body) # 把访问的得到的网页源码写入文件
```

里面的parse方法，这个方法有两个作用
1.负责解析start_url下载的Response 对象，根据item提取数据（解析item数据的前提是parse里全部requests请求都被加入了爬取队列）
2.如果有新的url则加入爬取队列，负责进一步处理，URL的Request 对象

* 启动scrapy爬虫

  1. 进入文件目录下

  > cd xxx

  2. 输入命令,启动爬虫

  >scrapy crawl dmoe

* 那么启动爬虫时发生了什么？

**Scrapy为Spider的 start_urls 属性中的每个url创建了Request 对象，并将 parse 方法作为回调函数(callback)赋值给了requests,而requests对象经过调度器的调度，执行生成response对象并送回给parse() 方法进行解析,所以请求链接的改变是靠回调函数实现的。**

### 三、Scrapy的整体架构和组成

官方的Scrapy的架构图



![](G:\文章写入文件夹\爬虫总结\scrapy.webp)

图中绿色的是数据的流向
我们看到图里有这么几个东西，分别是
Spiders：爬虫，定义了爬取的逻辑和网页内容的解析规则，主要负责解析响应并生成结果和新的请求
Engine：引擎，处理整个系统的数据流处理，出发事物，框架的核心。
Scheduler：调度器，接受引擎发过来的请求，并将其加入队列中，在引擎再次请求时将请求提供给引擎
Downloader：下载器，下载网页内容，并将下载内容返回给spider
ItemPipeline：项目管道，负责处理spider从网页中抽取的数据，主要是负责清洗，验证和向数据库中存储数据
Downloader Middlewares：下载中间件，是处于Scrapy的Request和Requesponse之间的处理模块
Spider Middlewares：spider中间件，位于引擎和spider之间的框架，主要处理spider输入的响应和输出的结果及新的请求middlewares.py里实现

***

是不感觉东西很多，很乱，有点懵！没关系，框架之所以是框架因为确实很简单
我们再来看下面的这张图！你就懂了！

![](G:\文章写入文件夹\爬虫总结\scrapy详情.webp)

***

- 最后我们来顺一下scrapy框架的整体执行流程：

> 1.spider的yeild将request发送给engine
> 2.engine对request不做任何处理发送给scheduler
> 3.scheduler，生成request交给engine
> 4.engine拿到request，通过middleware发送给downloader
> 5.downloader在\获取到response之后，又经过middleware发送给engine
> 6.engine获取到response之后，返回给spider，spider的parse()方法对获取到的response进行处理，解析出items或者requests
> 7.将解析出来的items或者requests发送给engine
> 8.engine获取到items或者requests，将items发送给ItemPipeline，将requests发送给scheduler（ps，只有调度器中不存在request时，程序才停止，及时请求失败scrapy也会重新进行请求）