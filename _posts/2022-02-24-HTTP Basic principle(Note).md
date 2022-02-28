---
title: 爬虫基础系列(一)-HTTP基本原理
categories:
- Notebook/web crawler
tags: 
---

> 网络爬虫技术相关笔记

相关链接：




# URL/URI

URI的全称为UniformResource Identifier，即统一资源标志符，URL的全称为Universal Resource Locator，即统一资源定位符。举例来说， http://github.com/favicon.ico是GitHub的网站图标链接，它是一个URL，也是一个URI。即有这样的一个图标资源，我们用URL/URI来唯一指定了它的访问方式，这其中包括了访问协议https、访问路径(/即根目录)和资源名称favicon.ico。通过这样一个链接，我们便可以从互联网上找到这个资源，这就是URL/URI。

# 超文本

超文本，英文名称为hypertext，网页就是超文本解析而成的， 其网页源代码是一系列HTML代码，里面包含了一系列标签。浏览器解析这些标签后，便形成了我们平常看到的网页。

# HTTP/HTTPS

在淘宝的首页https://www.taobao.com/中，URL的开头会有http或https，这就是访问资源需要的协议类型。有时，我们还会看到ftp、sftp、smb开头的URL，它们都是协议类型。在爬虫中，我们抓取的页面通常就是http或https协议的。

HTTP的全称是Hyper Text Transfer Protocol，中文名叫作超文本传输协议。HTTP协议是用于从网络传输超文本数据到本地浏览器的传送协议，它能保证高效而准确地传送超文本文档。HTTPS的全称是Hyper Text Transfer Protocol over Secure Socket Layer，是以安全为目标的HTTP通道，简单讲是HTTP的安全版，即HTTP下加入SSL层，简称为HTTPS。

SSL作用有两种：

- 建立一个信息安全通道来保证数据传输的安全；
- 确认网站的真实性。凡是使用了HTTPS的网站，都可以通过点击浏览器地址栏的锁头标志来查看网站认证之后的真实信息，也可以通过CA机构颁发的安全签章来查询。

# HTTP请求过程

在浏览器中输入一个URL，回车之后便会在浏览器中观察到页面内容。实际上，这个过程是浏览器向网站所在的服务器发送了一个请求，网站服务器接收到这个请求后进行处理和解析，然后返回对应的响应，接着传回浏览器。响应里包含了页面的源代码等内容，浏览器再对其进行解析，便将网页呈现了出来。

![1.png](https://Adamisyoung.github.io/assets/pic/HTTPBasicprinciple(Note)/1.png)

当输入bing的URL，回车后，可观察到发送请求和接收响应的过程。各列含义如下：

- Name：请求的名称，一般会将URL的最后一部分内容当作名称；
- Status：响应的状态码；
- Type：请求的文档类型。这里为document，代表这次请求的是一个HTML文档，内容就是一些HTML代码；
- Initiator：请求源，用来标记请求是由哪个对象或进程发起的；
- Size：从服务器下载的文件和请求的资源大小。如果是从缓存中取得的资源，则该列会显示from cache；
- Time：发起请求到获取响应所用的总时间；
- Waterfall：网络请求的可视化瀑布流。

点进去后可看到更详细的信息：

![2.png](https://Adamisyoung.github.io/assets/pic/HTTPBasicprinciple(Note)/2.png)

General中，Request URL为请求的URL，Request Method为请求的方法，Status Code为响应状态码，Remote Address为远程服务器的地址和端口，Referrer Policy为Referrer判别策略。

再继续往下，有Response Headers和Request Headers，这分别代表响应头和请求头。请求头里带有许多请求信息，例如浏览器标识、Cookies、Host等信息，这是请求的一部分，服务器会根据请求头内的信息判断请求是否合法，进而作出对应的响应。图中看到的Response Headers就是响应的一部分，例如其中包含了服务器的类型、文档类型、日期等信息。浏览器接受到响应后，会解析响应内容，进而呈现网页内容。

# 请求

请求，是由客户端向服务端发出。可以分为4部分内容：请求方法(Request Method)、请求的网址(Request URL)、请求头(Request Headers)、请求体(Request Body)。

## 请求方法

常见的请求方法有两种：GET和POST。

在浏览器中直接输入URL并回车，这便发起了一个GET请求，请求的参数会直接包含到URL里。例如，在百度中搜索Python，这就是一个GET请求，URL中会包含请求的参数信息。POST请求大多在表单提交时发起。比如，对于一个登录表单，输入用户名和密码后，点击“登录”按钮，这通常会发起一个POST请求，其数据通常以表单的形式传输，而不会体现在URL中。

GET和POST有如下区别：

- GET请求中的参数包含在URL里面，数据可以在URL中看到，而POST请求的URL不会包含这些数据，数据都是通过表单形式传输的，会包含在请求体中；
- GET请求提交的数据最多只有1024字节，而POST方式没有限制。

一般来说，登录时需要提交用户名和密码，其中包含了敏感信息，使用GET方式请求的话，密码就会暴露在URL里面，造成密码泄露。所以最好以POST方式发送。上传文件时，由于文件内容比较大，也会选用POST方式。

另外还有一些请求方法，一起总结如下：

| 方法 | 描述 |
| --- | --- |
| GET | 请求页面，并返回页面内容 |
| HEAD | 类似于GET请求，只不过返回的响应中没有具体的内容，用于获取报头 |
| POST | 大多用于提交表单或上传文件，数据包含在请求体中 |
| PUT | 从客户端向服务器传送的数据取代指定文梢中的内容 |
| DELETE | 请求服务器删除指定的页面 |
| CONNECT | 把服务器当作跳板，让服务器代替客户端防问其他网页 |
| OPTIONS | 允许客户端查看服务器的性能 |
| TRACE | 囚显服务器收到的请求，主要用于测试或诊断 |

## 请求的网址

请求的网址，即统一资源定位符URL，它可以唯一确定我们想请求的资源。

## 请求头

请求头，用来说明服务器要使用的附加信息，比较重要的信息有Cookie、Referer、User-Agent等。下面简要说明一些常用的头信息。

- Accept：请求报头域，用于指定客户端可接受哪些类型的信息；
- Accept-Language：指定客户端可接受的语言类型；
- Accept-Encoding：指定客户端可接受的内容编码；
- Host：用于指定请求资源的主机IP和端口号，其内容为请求URL的原始服务器或网关的位置。从HTTP1.1版本开始，请求必须包含此内容。
- Cookie：也常用复数形式Cookies，这是网站为了辨别用户进行会话跟踪而存储在用户本地的数据。它的主要功能是维持当前访问会话。例如我们输入用户名和密码成功登录某个网站后，服务器会用会话保存登录状态信息，后面我们每次刷新或请求该站点的其他页面时，会发现都是登录状态，这就是Cookies的功劳。Cookies里有信息标识了我们所对应的服务器的会话，每次浏览器在请求该站点的页面时，都会在请求头中加上Cookies并将其发送给服务器，服务器通过Cookies识别出是我们自己，并且查出当前状态是登录状态，所以返回结果就是登录之后才能看到的网页内容。
- Referer：此内容用来标识这个请求是从哪个页面发过来的，服务器可以拿到这一信息并做相应的处理，如做来源统计、防盗链处理等；
- User-Agent：简称UA，它是一个特殊的字符串头，可以使服务器识别客户使用的操作系统及版本、浏览器及版本等信息。在做爬虫时加上此信息，可以伪装为浏览器。如果不加，很可能会被识别为爬虫；
- Content-Type：也叫互联网媒体类型(Internet Media Type)或者MIME类型，在HTTP协议消息头中，它用来表示具体请求中的媒体类型信息。例如，text/html代表HTML格式，image/gif代表GIF图片，application/json代表JSON类型等等。

因此，请求头是请求的重要组成部分，在写爬虫时，大部分情况下都需要设定请求头。

## 请求体

请求体一般承载的内容是POST请求中的表单数据，而对于GET请求，请求体则为空。

例如，我在登录Github时捕获到的请求如下图所示：

![3.png](https://Adamisyoung.github.io/assets/pic/HTTPBasicprinciple(Note)/3.png)

登录之前，我填写了用户名和密码信息，提交时这些内容就会以表单数据的形式提交给服务器，此时需要注意Request Headers中指定Content-Type为application/x-www-form-urlencoded。只有设置Content-Type为application/x-www-form-urlencoded，才会以表单数据的形式提交。

下表列出了Content-Type和POST提交数据方式的关系：

| Content-Type | 提交数据的方式 |
| --- | --- |
| GETapplication/x-www-form-urlencoded | 表单数据 |
| multipart/form-data | 表单文件上传 |
| application/json | 序列化JSON数据 |
| text/xml | XML数据 |

在爬虫中，如果要构造POST请求，需要使用正确的Content-Type，并了解各种请求库的各个参数设置时使用的是哪种Content-Type，不然可能会导致POST提交后无法正常响应。

# 响应

响应，由服务端返回给客户端，可以分为三部分：响应状态码(Response Status Code)、响应头(Response Headers)和响应体(Response Body)。

## 响应状态码

响应状态码表示服务器的响应状态，如200代表服务器正常响应，404代表页面未找到，500代表服务器内部发生错误。在爬虫中，我们可以根据状态码来判断服务器响应状态，如状态码为200，则证明成功返回数据，再进行进一步的处理，否则直接忽略。

## 响应头

响应头包含了服务器对请求的应答信息，如Content-Type、Server、Set-Cookie等。

- Date：标识响应产生的时间；
- Last-Modified：指定资源的最后修改时间；
- Content-Encoding：指定响应内容的编码；
- Server：包含服务器的信息，比如名称、版本号等；
- Content-Type：文档类型，指定返回的数据类型是什么，如text/html代表返回HTML文档，application/x-javascript则代表返回JavaScript文件，image/jpeg则代表返回图片。
- Set-Cookie：设置Cookies。响应头中的Set-Cookie告诉浏览器需要将此内容放在Cookies中，下次请求携带Cookies请求；
- Expires：指定响应的过期时间，可以使代理服务器或浏览器将加载的内容更新到缓存中。如果再次访问时，就可以直接从缓存中加载，降低服务器负载，缩短加载时间。

## 响应体

最重要的当属响应体的内容了。响应的正文数据都在响应体中，比如请求网页时，它的响应体就是网页的HTML代码；请求一张图片时，它的响应体就是图片的二进制数据。我们做爬虫请求网页后，要解析的内容就是响应体。

在浏览器开发者工具中点击Preview，就可以看到网页的源代码，也就是响应体的内容，它是解析的目标。在做爬虫时，我们主要通过响应体得到网页的源代码、JSON数据等，然后从中做相应内容的提取。