---
title: Selenium随记(一)
copyright: true
mathjax: true
categories:
- Notebook/Selenium
tags: 
---
> 和浏览器发生别样的互动～

参考：[https://www.selenium.dev/documentation/](https://www.selenium.dev/documentation/)

Selenium通过使用*WebDriver*支持市场上所有主流(major)浏览器的自动化(automation)。Webdriver是一个API和协议(protocol)，是W3C推荐标准([https://www.w3.org/TR/webdriver1/](https://www.w3.org/TR/webdriver1/))。它定义了一个语言中立(language-neutral)的接口(interface)，用于控制web浏览器的行为。每个浏览器都有一个特定的WebDriver实现(specific WebDriver implementation)，称为驱动程序。驱动程序是负责委派给浏览器的组件(delegating down to the browser)，并处理与Selenium和浏览器之间的通信。

形象的说，Webdriver通过一个驱动程序与浏览器对话。对话有两种方式：直接通信和远程通信。

它们的区别就是Webdriver的位置不同。如下面两张图的区别一样。

![1.png](https://Adamisyoung.github.io/assets/pic/Seleniumnotes1/1.png)

![2.png](https://Adamisyoung.github.io/assets/pic/Seleniumnotes1/2.png)

Selenium的安装略，我常用Google浏览器，所以并不涉及其他浏览器。

一般浏览器会自动更新，但驱动程序不会。所以为了方便驱动程序的版本管理，可以import [WebDriver Manager for Python](https://github.com/SergeyPirogov/webdriver_manager)：

```python
# selenium 4
from webdriver_manager.chrome import ChromeDriverManager
from selenium import webdriver
from selenium.webdriver import ChromeOptions
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
```

通过install()函数获得使用位置并将其传递给服务类(pass it into service class)

通常浏览器在特定选项下启动。这些选项描述浏览器所支持的功能，以及在会话期间浏览器的行为。有些功能是所有浏览器通用，而有些功能则只在特定浏览器上使用。示例脚本参考[https://github.com/SeleniumHQ/seleniumhq.github.io/blob/dev/examples/python/tests/getting_started/test_open_browser.py#L9-L12](https://github.com/SeleniumHQ/seleniumhq.github.io/blob/dev/examples/python/tests/getting_started/test_open_browser.py#L9-L12)。

驱动部分解决后就可以正式写Selenium脚本了。Selenium所做的一切本质上是给**浏览器发送一串命令，用来执行某些操作或是发送请求**。

首先使用实例开启会话，输入一段URL进入相应的网页中。下方脚本关于driver的注释是老用法，将驱动放在工程根目录下执行，但这样不如webdriver_manager做统一管理来的方便。

```python
# selenium 4
from selenium import webdriver
from selenium.webdriver import ChromeOptions
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

service = Service(ChromeDriverManager().install())
option = ChromeOptions()
driver = webdriver.Chrome(service=service, options=option)
# driver = webdriver.Chrome(executable_path='./chromedriver', options=option)
driver.get('https://www.google.com')
driver.maximize_window()
driver.quit()
```

脚本的效果是打开Google，然后关闭(quit结束进程，也会关掉浏览器)。log输出说明能正常检测到在缓冲区的驱动程序。

```python
====== WebDriver manager ======
Current google-chrome version is 98.0.4758
Get LATEST chromedriver version for 98.0.4758 google-chrome
Driver [/Users/zhanghaowei/.wdm/drivers/chromedriver/mac64_m1/98.0.4758.102/chromedriver] found in cache
```

如果使用的是注释的用法的话，log会提醒executable_path已经濒临弃用状态，所以还是用新方法较好。

上面的脚本并没有让页面停留的逻辑，而将当前的代码与浏览器之间的状态同步(Synchronize)是Selenium的最大挑战之一，直接影响使用体验。这层交互本质上(Essentially)是在定位网页元素前，我们希望元素一直在页面上，并确保其处于可交互状态(in an interactable state)。

常见的一些停留手段，标准库可以import time，然后time.sleep()；webdriver也有函数：driver.implicitly_wait()。这些都是隐式的等待方式(An implicit wait)，并不是一个解决问题的好方法，唯一的好处就是实现起来非常简单。