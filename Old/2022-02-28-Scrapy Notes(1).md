---
title: Scrapy随记(一)
copyright: true
mathjax: true
categories:
- Notebook/Scrapy
tags: 
---

> 项目有数据需求，所以学习一下Scrapy框架。

参考文档：

[Scrapy Tutorial - Scrapy 2.5.1 documentation](https://docs.scrapy.org/en/latest/intro/tutorial.html)

下载Scrapy：

```bash
pip3 install scrapy
```

初始化Scrapy工程：

```bash
scrapy startproject tutorial
```

生成的文件目录标记如下：

```bash
[tutorial] tree -a                                                    13:04:25 
.
├── scrapy.cfg  
└── tutorial  # 存放Python模块的目录
    ├── __init__.py
    ├── items.py  # 项目定义文件
    ├── middlewares.py # 中间件文件
    ├── pipelines.py # 管道文件
    ├── settings.py # 设置文件
    └── spiders   # 存放爬虫的目录
        └── __init__.py

2 directories, 7 files
```

在spiders文件夹内创建第一个爬虫，因为要爬取`http://quotes.toscrape.com`的内容，所以命名为`qutoes_spider.py`。

```python
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"  

    def start_requests(self):
        urls = [
            'http://quotes.toscrape.com/page/1/',
            'http://quotes.toscrape.com/page/2/',
        ]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse)

    def parse(self, response):
				page = response.url.split("/")[-2]
        filename = f'quotes-{page}.html'
        with open(filename, 'wb') as f:
            f.write(response.body)
        self.log(f'Saved file {filename}')

        # 运行后就会有两个html文件生成，内部是爬取的内容。正如parse方法所指示(instruct)的那样，使用相应(respective)URL的内容(content)。
```

创建`QuotesSpider`方法，我们的爬虫文件继承(subclass)了scrapy.Spider，并定义了一些属性和方法。

- `name`：爬虫蜘蛛的名称，一个项目内是唯一(Unique)的；
- `start_requests()`：必须返回一个可迭代(iterable)的Requests(针对此操作，你可以返回一个请求列表或编写一个生成器函数)，Spider将从该迭代开始爬取(crawl)，后续请求(subsequent requests)将从这些初始请求(initial requests)中依次生成；
- `parse()`：此方法的作用是处理(handle)每个请求下载的响应。response参数(parameter)是TextResponse的一个实例(instance)，它保存页面内容(page content)并具有更多(further)有用的方法来处理(handle)它。
    
    parse()方法通常解析(parse)响应(response)，将抓取的数据提取为dicts，并找到要遵循的新URL并从中创建新请求(Request)。
    

运行下面命令即可运行：

```bash
scrapy crawl quotes
```

该命令使用了我们新创建的蜘蛛，将对quotes.toscrape.com发送请求。输出如下：

```bash
> scrapy crawl quotes         
2022-02-28 12:54:25 [scrapy.utils.log] INFO: Scrapy 2.5.1 started (bot: tutorial)
2022-02-28 12:54:25 [scrapy.utils.log] INFO: Versions: lxml 4.8.0.0, libxml2 2.9.4, cssselect 1.1.0, parsel 1.6.0, w3lib 1.22.0, Twisted 22.1.0, Python 3.10.0 (default, Dec 11 2021, 07:07:40) [Clang 13.0.0 (clang-1300.0.29.3)], pyOpenSSL 22.0.0 (OpenSSL 1.1.1m  14 Dec 2021), cryptography 36.0.1, Platform macOS-12.0.1-arm64-arm-64bit
2022-02-28 12:54:25 [scrapy.utils.log] DEBUG: Using reactor: twisted.internet.selectreactor.SelectReactor
2022-02-28 12:54:25 [scrapy.crawler] INFO: Overridden settings:
{'BOT_NAME': 'tutorial',
 'NEWSPIDER_MODULE': 'tutorial.spiders',
 'ROBOTSTXT_OBEY': True,
 'SPIDER_MODULES': ['tutorial.spiders']}
2022-02-28 12:54:25 [scrapy.extensions.telnet] INFO: Telnet Password: 2ec45bf575e02daf
2022-02-28 12:54:26 [scrapy.middleware] INFO: Enabled extensions:
['scrapy.extensions.corestats.CoreStats',
 'scrapy.extensions.telnet.TelnetConsole',
 'scrapy.extensions.memusage.MemoryUsage',
 'scrapy.extensions.logstats.LogStats']
2022-02-28 12:54:26 [scrapy.middleware] INFO: Enabled downloader middlewares:
['scrapy.downloadermiddlewares.robotstxt.RobotsTxtMiddleware',
 'scrapy.downloadermiddlewares.httpauth.HttpAuthMiddleware',
 'scrapy.downloadermiddlewares.downloadtimeout.DownloadTimeoutMiddleware',
 'scrapy.downloadermiddlewares.defaultheaders.DefaultHeadersMiddleware',
 'scrapy.downloadermiddlewares.useragent.UserAgentMiddleware',
 'scrapy.downloadermiddlewares.retry.RetryMiddleware',
 'scrapy.downloadermiddlewares.redirect.MetaRefreshMiddleware',
 'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware',
 'scrapy.downloadermiddlewares.redirect.RedirectMiddleware',
 'scrapy.downloadermiddlewares.cookies.CookiesMiddleware',
 'scrapy.downloadermiddlewares.httpproxy.HttpProxyMiddleware',
 'scrapy.downloadermiddlewares.stats.DownloaderStats']
2022-02-28 12:54:26 [scrapy.middleware] INFO: Enabled spider middlewares:
['scrapy.spidermiddlewares.httperror.HttpErrorMiddleware',
 'scrapy.spidermiddlewares.offsite.OffsiteMiddleware',
 'scrapy.spidermiddlewares.referer.RefererMiddleware',
 'scrapy.spidermiddlewares.urllength.UrlLengthMiddleware',
 'scrapy.spidermiddlewares.depth.DepthMiddleware']
2022-02-28 12:54:26 [scrapy.middleware] INFO: Enabled item pipelines:
[]
2022-02-28 12:54:26 [scrapy.core.engine] INFO: Spider opened
2022-02-28 12:54:26 [scrapy.extensions.logstats] INFO: Crawled 0 pages (at 0 pages/min), scraped 0 items (at 0 items/min)
2022-02-28 12:54:26 [scrapy.extensions.telnet] INFO: Telnet console listening on 127.0.0.1:6023
2022-02-28 12:54:28 [scrapy.core.engine] DEBUG: Crawled (404) <GET http://quotes.toscrape.com/robots.txt> (referer: None)
2022-02-28 12:54:30 [scrapy.core.engine] DEBUG: Crawled (200) <GET http://quotes.toscrape.com/page/1/> (referer: None)
2022-02-28 12:54:30 [scrapy.core.engine] DEBUG: Crawled (200) <GET http://quotes.toscrape.com/page/2/> (referer: None)
2022-02-28 12:54:30 [quotes] DEBUG: Saved file quotes-1.html
2022-02-28 12:54:30 [quotes] DEBUG: Saved file quotes-2.html
2022-02-28 12:54:30 [scrapy.core.engine] INFO: Closing spider (finished)
2022-02-28 12:54:30 [scrapy.statscollectors] INFO: Dumping Scrapy stats:
{'downloader/request_bytes': 681,
 'downloader/request_count': 3,
 'downloader/request_method_count/GET': 3,
 'downloader/response_bytes': 5663,
 'downloader/response_count': 3,
 'downloader/response_status_count/200': 2,
 'downloader/response_status_count/404': 1,
 'elapsed_time_seconds': 3.881814,
 'finish_reason': 'finished',
 'finish_time': datetime.datetime(2022, 2, 28, 4, 54, 30, 184164),
 'httpcompression/response_bytes': 24787,
 'httpcompression/response_count': 2,
 'log_count/DEBUG': 5,
 'log_count/INFO': 10,
 'memusage/max': 60882944,
 'memusage/startup': 60882944,
 'response_received_count': 3,
 'robotstxt/request_count': 1,
 'robotstxt/response_count': 1,
 'robotstxt/response_status_count/404': 1,
 'scheduler/dequeued': 2,
 'scheduler/dequeued/memory': 2,
 'scheduler/enqueued': 2,
 'scheduler/enqueued/memory': 2,
 'start_time': datetime.datetime(2022, 2, 28, 4, 54, 26, 302350)}
2022-02-28 12:54:30 [scrapy.core.engine] INFO: Spider closed (finished)
```

项目根目录会多出两个html文件，正如parse方法所指示(instruct)的那样，使用相应(respective)URL的内容(content)。

![1.png](https://Adamisyoung.github.io/assets/pic/Scrapynotes1/1.png)

Scrapy调度(schedules)Spider的start_requests方法(methods)返回的scrapy.Request对象(objects)。在收到每个响应时，它会实例化(nstantiates)Response对象并调用与请求关联(associated)的回调方法(本例中为parse 方法)，将响应作为参数传递。

其实我们无需实现(implement)从URL处生成scrapy.Request对象的start_requests()方法，只需定义一个带有URL列表的start_urls类属性。然后这个列表将被start_requests()的默认实现用来为Spider创建初始请求。

```python
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"  # 一个项目内是唯一(Unique)的
    start_urls = [
		    'http://quotes.toscrape.com/page/1/',
        'http://quotes.toscrape.com/page/2/',
    ]

    def parse(self, response):
        # 调用此函数为处理每个请求下载的响应的方法。
        # response参数是TextResponse的一个实例(instance)，它保存页面内容(page content)并具有更多(further)有用的方法来处理(handle)它。
        # parse()方法通常解析(parse)响应(response)，将抓取的数据提取为dicts，并找到要遵循的新URL并从中创建新请求(Request)。
        page = response.url.split("/")[-2]
        filename = f'quotes-{page}.html'
        with open(filename, 'wb') as f:
            f.write(response.body)
        self.log(f'Saved file {filename}')
```

即便我们没有明确(explicitly)告诉Scrapy这样做，parse()方法依旧可以正确处理那些请求。这是因为parse()是Scrapy的默认回调方法，可以在没有显式分配回调(explicitly assigned callback)的情况下正常调用。

利用Scrapy提取数据的最佳方法是使用Scrapy shell的选择器。通过shell，我们可以使用带有response object的css来选择元素。

打开我们爬取下来的HTML文档，http://quotes.toscrape.com中的每个quote都由如下所示的HTML元素表示：

```html
<div class="quote">
    <span class="text">“The world as we have created it is a process of our
    thinking. It cannot be changed without changing our thinking.”</span>
    <span>
        by <small class="author">Albert Einstein</small>
        <a href="/author/Albert-Einstein">(about)</a>
    </span>
    <div class="tags">
        Tags:
        <a class="tag" href="/tag/change/page/1/">change</a>
        <a class="tag" href="/tag/deep-thoughts/page/1/">deep-thoughts</a>
        <a class="tag" href="/tag/thinking/page/1/">thinking</a>
        <a class="tag" href="/tag/world/page/1/">world</a>
    </div>
</div>
```

现在打开Scrapy shell，根据提示来进行数据提取：

```bash
> scrapy shell 'http://quotes.toscrape.com/page/1/'

2022-02-28 13:45:06 [scrapy.utils.log] INFO: Scrapy 2.5.1 started (bot: tutorial)
2022-02-28 13:45:06 [scrapy.utils.log] INFO: Versions: lxml 4.8.0.0, libxml2 2.9.4, cssselect 1.1.0, parsel 1.6.0, w3lib 1.22.0, Twisted 22.1.0, Python 3.10.0 (default, Dec 11 2021, 07:07:40) [Clang 13.0.0 (clang-1300.0.29.3)], pyOpenSSL 22.0.0 (OpenSSL 1.1.1m  14 Dec 2021), cryptography 36.0.1, Platform macOS-12.0.1-arm64-arm-64bit
2022-02-28 13:45:06 [scrapy.utils.log] DEBUG: Using reactor: twisted.internet.selectreactor.SelectReactor
2022-02-28 13:45:06 [scrapy.crawler] INFO: Overridden settings:
{'BOT_NAME': 'tutorial',
 'DUPEFILTER_CLASS': 'scrapy.dupefilters.BaseDupeFilter',
 'LOGSTATS_INTERVAL': 0,
 'NEWSPIDER_MODULE': 'tutorial.spiders',
 'ROBOTSTXT_OBEY': True,
 'SPIDER_MODULES': ['tutorial.spiders']}
2022-02-28 13:45:06 [scrapy.extensions.telnet] INFO: Telnet Password: 16268c94c14a4fe1
2022-02-28 13:45:06 [scrapy.middleware] INFO: Enabled extensions:
['scrapy.extensions.corestats.CoreStats',
 'scrapy.extensions.telnet.TelnetConsole',
 'scrapy.extensions.memusage.MemoryUsage']
2022-02-28 13:45:06 [scrapy.middleware] INFO: Enabled downloader middlewares:
['scrapy.downloadermiddlewares.robotstxt.RobotsTxtMiddleware',
 'scrapy.downloadermiddlewares.httpauth.HttpAuthMiddleware',
 'scrapy.downloadermiddlewares.downloadtimeout.DownloadTimeoutMiddleware',
 'scrapy.downloadermiddlewares.defaultheaders.DefaultHeadersMiddleware',
 'scrapy.downloadermiddlewares.useragent.UserAgentMiddleware',
 'scrapy.downloadermiddlewares.retry.RetryMiddleware',
 'scrapy.downloadermiddlewares.redirect.MetaRefreshMiddleware',
 'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware',
 'scrapy.downloadermiddlewares.redirect.RedirectMiddleware',
 'scrapy.downloadermiddlewares.cookies.CookiesMiddleware',
 'scrapy.downloadermiddlewares.httpproxy.HttpProxyMiddleware',
 'scrapy.downloadermiddlewares.stats.DownloaderStats']
2022-02-28 13:45:06 [scrapy.middleware] INFO: Enabled spider middlewares:
['scrapy.spidermiddlewares.httperror.HttpErrorMiddleware',
 'scrapy.spidermiddlewares.offsite.OffsiteMiddleware',
 'scrapy.spidermiddlewares.referer.RefererMiddleware',
 'scrapy.spidermiddlewares.urllength.UrlLengthMiddleware',
 'scrapy.spidermiddlewares.depth.DepthMiddleware']
2022-02-28 13:45:06 [scrapy.middleware] INFO: Enabled item pipelines:
[]
2022-02-28 13:45:06 [scrapy.extensions.telnet] INFO: Telnet console listening on 127.0.0.1:6023
2022-02-28 13:45:06 [scrapy.core.engine] INFO: Spider opened
2022-02-28 13:45:07 [scrapy.core.engine] DEBUG: Crawled (404) <GET http://quotes.toscrape.com/robots.txt> (referer: None)
2022-02-28 13:45:08 [scrapy.core.engine] DEBUG: Crawled (200) <GET http://quotes.toscrape.com/page/1/> (referer: None)
[s] Available Scrapy objects:
[s]   scrapy     scrapy module (contains scrapy.Request, scrapy.Selector, etc)
[s]   crawler    <scrapy.crawler.Crawler object at 0x10527a290>
[s]   item       {}
[s]   request    <GET http://quotes.toscrape.com/page/1/>
[s]   response   <200 http://quotes.toscrape.com/page/1/>
[s]   settings   <scrapy.settings.Settings object at 0x10527a9b0>
[s]   spider     <DefaultSpider 'default' at 0x1059e07f0>
[s] Useful shortcuts:
[s]   fetch(url[, redirect=True]) Fetch URL and update local objects (by default, redirects are followed)
[s]   fetch(req)                  Fetch a scrapy.Request and update local objects 
[s]   shelp()           Shell help (print this help)
[s]   view(response)    View response in a browser
```

我们可以得到一个quote HTML元素的选择器列表：

```bash
>>> response.css("div.quote")
[<Selector xpath="descendant-or-self::div[@class and contains(concat(' ', normalize-space(@class), ' '), ' quote ')]" data='<div class="quote" itemscope itemtype...'>,
 <Selector xpath="descendant-or-self::div[@class and contains(concat(' ', normalize-space(@class), ' '), ' quote ')]" data='<div class="quote" itemscope itemtype...'>, 
...
```

运行response.css("title")返回的结果是一个名为SelectorList的对象。它和列表类似，表示XML/HTML元素的Selector对象列表，并允许进一步的查询以细化(fine-gain)选择或提取数据。

上面查询(query)返回的每个选择器都允许我们对其子元素运行进一步(further)的查询。我们先将其中一个(这里直接将第一个)选择器分配(assign)给一个变量，这样我们就可以直接在特定的quote上运行CSS选择器：

```bash
>>> quote = response.css("div.quote")[0]
```

创建好后，我们可以从该选择器对象中提取(extract)出文本、作者和标签：

```bash
>>> quote = response.css("div.quote")[0]
>>> text = quote.css("span.text::text").get()
>>> text
'“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”'
>>> author = quote.css("small.author::text").get()
>>> author
'Albert Einstein'
```

鉴于标签是字符串列表，我们可以使用getall()方法获取所有标签：

```bash
>>> tags = quote.css("div.tags a.tag::text").getall()
>>> tags
['change', 'deep-thoughts', 'thinking', 'world']
```

以上是提取每个选择器信息的过程，我们可以遍历所有quote元素来提取它们。之后的工作就在脚本中完成。

之前编写的脚本只是将整个HTML page保存下来，并没有进行信息提取。Spider通常会生成很多字典，其中包含从页面提取的数据。所以我们在回调中使用yield关键字。

```python
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"
    start_urls = [
            'http://quotes.toscrape.com/page/1/',
            'http://quotes.toscrape.com/page/2/',
        ]

    def parse(self, response):
        for quote in response.css('div.quote'):
            yield {
                'text': quote.css('span.text::text').get(),
                'author': quote.css('small.author::text').get(),
                'tags': quote.css('div.tags a.tag::text').getall(),
            }
```

运行后会输出下方的log：

```python
2022-02-28 16:06:33 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/page/1/>
{'text': '“There are only two ways to live your life. One is as though nothing is a miracle. The other is as though everything is a miracle.”', 'author': 'Albert Einstein', 'tags': ['inspirational', 'life', 'live', 'miracle', 'miracles']}
2022-02-28 16:06:33 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/page/1/>
...
```

提取数据的下一步是存储(store)。最简单的方法是使用Feed exports：

```bash
scrapy crawl quotes -O quotes.json
```

这将生成一个quotes.json文件，其中包含所有抓取的项目，并以JSON序列化(serialized in JSON)。-0选项是覆盖现有文件，如果改用-o是将新内容附加(append)到现有文件。不过后者会使JSON文件内容失效，所以如果使用-o选项，就需要考虑使用其他的序列化格式，如JSON Lines。

在小型项目中这样存储已然足够，如果是想做一些略微复杂的事情，就需要编写相应的管道(pipeline)。在项目生成之初，就在tutorial/pipelines.py中自动设置了Item Pipelines的占位符文件。