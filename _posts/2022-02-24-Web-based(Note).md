---
title: 爬虫基础系列(二)-网页基础
categories:
- Notebook/web crawler
tags: 
---

> 网络爬虫技术相关笔记。如果把网页比作一个人的话，HTML相当于骨架，JavaScript相当于肌肉，CSS相当于皮肤。三者结合起来才能形成一个完善的网页。

网页分为三大部分：HTML，CSS和JavaScript。
# 网页的组成

## HTML

HTML是用来描述网页的一种语言，其全称叫作Hyper Text Markup Language，即超文本标记语言。网页包括文字、按钮、图片和视频等各种复杂的元素，其基础架构就是HTML。不同类型的文字通过不同类型的标签来表示，如图片用img标签表示，视频用video标签表示，段落用p标签表示，它们之间的布局又常通过布局标签div嵌套组合而成，各种标签通过不同的排列和嵌套才形成了网页的框架。

![1.png](https://Adamisyoung.github.io/assets/pic/WebBased(Note)/1.png)

在Elements选项卡中可看到网页的源代码。

## CSS

HTML定义了网页的结构，但是只有HTML页面的布局并不美观，可能只是简单的节点元素的排 列，为了让网页看起来更好看一些，这里借助了CSS。

CSS，全称叫作Cascading Style Sheets，即层叠样式表。“层叠”是指当在HTML中引用了数个样式文件，并且样式发生冲突时，浏览器能依据层叠顺序处理。“样式”指网页中文字大小、颜色、元素间距、排列等格式。

CSS是目前唯一的网页页面排版样式标准，有了它的帮助，页面才会变得更为美观。

我从Github那里截取一段CSS：

```css
@media (prefers-color-scheme: light)
[data-color-mode=auto][data-light-theme*=light] {
    --color-workflow-card-connector: var(--color-scale-gray-3);
    --color-workflow-card-connector-bg: var(--color-scale-gray-3);
    --color-workflow-card-connector-inactive: var(--color-border-default);
    --color-workflow-card-connector-inactive-bg: var(--color-border-default);
    --color-workflow-card-connector-highlight: var(--color-scale-blue-4);
    --color-workflow-card-connector-highlight-bg: var(--color-scale-blue-4);
    --color-workflow-card-bg: var(--color-scale-white);
    --color-workflow-card-inactive-bg: var(--color-canvas-inset);
    --color-workflow-card-header-shadow: rgba(0, 0, 0, 0);
    --color-workflow-card-progress-complete-bg: var(--color-scale-blue-4);
    --color-workflow-card-progress-incomplete-bg: var(--color-scale-gray-2);
    --color-discussions-state-answered-icon: var(--color-scale-white);
    --color-bg-discussions-row-emoji-box: rgba(209, 213, 218, 0.5);
    --color-notifications-button-text: var(--color-fg-muted);
    --color-notifications-button-hover-text: var(--color-fg-default);
    --color-notifications-button-hover-bg: var(--color-scale-gray-2);
    --color-notifications-row-read-bg: var(--color-canvas-subtle);
    --color-notifications-row-bg: var(--color-scale-white);
    --color-icon-directory: var(--color-scale-blue-3);
    --color-checks-step-error-icon: var(--color-scale-red-4);
    --color-calendar-halloween-graph-day-L1-bg: #ffee4a;
    --color-calendar-halloween-graph-day-L2-bg: #ffc501;
    --color-calendar-halloween-graph-day-L3-bg: #fe9600;
    --color-calendar-halloween-graph-day-L4-bg: #03001c;
    --color-calendar-graph-day-bg: #ebedf0;
    --color-calendar-graph-day-border: rgba(27, 31, 35, 0.06);
    --color-calendar-graph-day-L1-bg: #9be9a8;
    --color-calendar-graph-day-L2-bg: #40c463;
    --color-calendar-graph-day-L3-bg: #30a14e;
    --color-calendar-graph-day-L4-bg: #216e39;
    --color-calendar-graph-day-L1-border: rgba(27, 31, 35, 0.06);
    --color-calendar-graph-day-L2-border: rgba(27, 31, 35, 0.06);
    --color-calendar-graph-day-L3-border: rgba(27, 31, 35, 0.06);
    --color-calendar-graph-day-L4-border: rgba(27, 31, 35, 0.06);
    --color-user-mention-fg: var(--color-fg-default);
    --color-user-mention-bg: var(--color-attention-subtle);
    --color-text-white: var(--color-scale-white);
}
```

@media是一种样式，大括号内部就是一条条样式规则。在网页中，一般会统一定义整个网页的样式规则，并写人css文件中。在HTML中，只需要用link标签即可引人写好的css文件，这样整个页面就会变得美观、 优雅。

## JavaScript

JavaScript是一种脚本语言。HTML和css配合使用，提供给用户的只是一种静态信息，缺乏交互性。我们在网页里可能会看到一些交互和动画效果，如下载进度条、提示框、轮播图等，这通常就是JavaScript的功劳。它的出现使得用户与信息之间不只是一种浏览与显示的关系，而是实现了一种实时、动态、交互的页面功能。

JavaScript通常也是以单独的文件形式加载的，后缀为js，在HTML中通过script标签即可引入。

综上所述，HTML定义了网页的内容和结构，css描述了网页的布局，JavaScript定义了网页的行为。

# HTML节点树及节点间关系

在HTML中，所有标签定义的内容都是节点，它们构成了一个HTML DOM树。DOM是W3C的标准，其英文全称为Document Object Model，即文档对象模型。它定义了访问HTML和XML文档的标准：DOM是中立于平台和语言的接口，它允许程序和脚本动态访问和更新文档的内容、结构和样式。

W3C DOM标准被分为3个不同的部分：

- 核心DOM：针对任何结构化文档的标准模型；
- XML DOM：针对XML文挡的标准模型；
- HTML DOM：针对HTML文档的标准模型。

根据标准，HTML文档中所有内容都是节点：

- 整个文档是一个文档节点；
- 每个HTML元素是元素节点；
- HTML元素内的文本是文本节点；
- 每个HTML属性是属性节点；
- 注释是注释节点。

HTML DOM将HTML文档视作树结构，这种结构被称为节点树。

![2.png](https://Adamisyoung.github.io/assets/pic/WebBased(Note)/2.png)

通过HTML DOM，树中的所有节点均可通过JavaScript访问，所有HTML节点元素均可被修改，也可以被创建或删除。节点树中的节点彼此拥有层级关系。我们常用父(parent)、子(child)和兄弟(sibling)等术语描述这些关系。父节点拥有子节点，同级的子节点被称为兄弟节点。在节点树中，顶端节点称为根(root)。除了根节点之外，每个节点都有父节点，同时可拥有任意数量的子节点或兄弟节点。术语基本都是类似的。

# 选择器

网页由一个个节点组成，CSS选择器会根据不同的节点设置不同的样式规则，那我们可以使用CSS选择器来定位节点。另外，CSS选择器还支持嵌套选择，各个选择器之间加上空格分隔开便可以代表嵌套关系。如果不加空格，则代表并列关系。

还有一种比较常用的选择器是XPath。