---
title: 长江经济带产业链知识图谱-总体设计篇
copyright: true
mathjax: true
categories:
- Graduation Project/Artificial Intelligence
tags: 
---

> 本课题尽可能从基础性研究做起，发挥NLP领域的文本分析特长，将研究成果应用于政府、区域、企业的产业链、集群、上下游推荐，以及产业链的延链、固链、补链等问题上。

# 课题来源及研究的目的和意义

## 概念

产业链是产业经济学中的一个概念，是一个包含**价值链**、**企业链**、**供需链**和**空间链**四个维度的概念。它是各个产业部分之间基于一定的技术经济关联，并依据特定的**逻辑关系和时空布局关系**客观形成的链条式关联关系形态。随着技术的发展，迂回生产程度的提高，生产过程可以划分为一系列有**关联的生产环节**，企业难以应付越来越复杂的分工与交易活动，不得不依靠企业之间的相互关联，这种搜寻最佳企业组织结构的动力与实践就成为产业链形成的条件。社会分工是市场交易的起点，也是产业链产生的起点。随着市场交易程度的不断加深，产业链条也在不断延伸，形式日益复杂。**区域经济最终以特色集群的形式展现区域核心竞争力**。

产业链按照结构属性也分为**接通产业链**和**延伸产业链**。接通产业链是指将一定地域空间范围内的断续的产业部门(通常是产业链的断环和孤环)借助某种产业合作的形式串联起来；延伸产业链则是将一条即已存在的产业链尽可能地向**上下游拓深延展**。产业链向上游延伸一般使得产业链进入到基础产业环节和技术研发环节，向下游拓深则进入到市场拓展环节。而这种产业关联的实质则是各产业中的企业之间的**供给与需求**的关系。这种上下游关系和相互价值的交换是大量存在的，上游环节向下游环节输送产品或服务，下游环节向上游环节反馈信息。

## 意义

产业链是很多概念的影响因素。产业链和工业互联网有关，和区域经济有关，甚至和中美产业链对抗的贸易战也是有关的。现代战争的形势往往是温和而残酷的，产业链也可以变成双方王对王，将对将的战场。拼的就是各自产业集群在全球产业链的内力，彼此攻击对方的供应链，将对方从全球产业链中弱化。产业链并不是一个地方区域经济的终极形态，它只是一个行业的产业上下游分工情况。而集群则是某个区域在某个产业链上建立了优势地位，从而成为本区域的优势产业。产业链是僵死的概念分类，集群是鲜活的企业群体。如果一个国家、区域不能利用算法去精确计算产业链和集群情况，那么在相关领域的对抗中会多出很多不稳定的因素。如果我们在大数据的支撑下，能精准分析出产业链分布和集群分布，那么就能为国家、区域的一些经济决策提供定量分析手段，算是一把利器。

目前东北经济连年衰弱，沿海城市守成有余还开疆破土，这一系列的现象背后是有内在的原因的。现在的战争或是竞争，已经不再是企业之间的竞争，而是双方的产业链、集群之间的对抗和竞争。即群体之间的竞争，而不再是个体之间的竞争。产业链、集群的概念本身也在强调了群体的竞争力。企业，集群在某种意义上都是生命体，它们都需要有共生的生态环境，才能避免消亡。正因如此，南方沿海城市一直在发展产业集群，产业链招商，经营营商环境，而北方，区域特色经济由于管理不佳或遭到破坏，已经在逐步的衰弱。在管理思想上就已经有天然的差距。一个产业链壮大的背后，离不开龙头企业和集群网络的支持。

这也就是互联网公司受政策影响极大的原因。互联网产业并没有产业链架构，当国家开始整治时，这些企业都是很脆弱的。尽管它们市值庞大，一旦受到政策调控，就可能变为负资产。它们之间并没有产业链的依存关系。而华为发展多条产业链，和各行各业都形成了产业链的依存关系，所以不会轻易就被制裁。大疆也是类似，大疆做到了产业链自给自足，美国制裁也打不死。它们如果倒了，产业链就会遭受到极大冲击，而互联网公司这种依靠资本快速催生的产物，并没有在产业链有稳固地位，没有集群的共生依存的保护，哪怕倒下很快也会有替代品。可见集群对于企业就像铠甲一样，共生共死。

产业链安全稳定是国家构建新发展格局的重要保障，故本毕设应用自然语言处理和软件工程思想，从产业链的四个维度入手，以**长江经济带**为分析目标，构建一个多层次多维度产业链知识图谱，为监管、投资、融资和招商等应用领域提供服务，能**自动分析特定行业、特定地区的产业链和集群情况**。对于战略新兴行业，像半导体，电动车，可以建立产业链图谱；对于特定地区，可以自动发现它的一些主导性产业集群。

虽然我对产业网络，企业网络经验不足，但是究其本质，还是能抓住它的核心价值：链和群。链就是网络上的路径，群就是网络上的社区。有了链，世界是泛链的，万物互联，万企互联。任意两个企业之间都有路径，存在着潜在的供应链关系。有了群，就有企业集群，商圈。这是网络本质的归因。如果从经济学意义看，也是如此：全球化的产业分工所对应的产业链和区域经济竞争所形成的集群。各有千秋。

本课题尽可能从基础性研究做起，发挥NLP领域的文本分析特长，将研究成果应用于政府、区域、企业的产业链、集群、上下游推荐，以及产业链的延链、固链、补链等问题上。

# 国内外在该方向的研究现状及分析

计算机始终面临着这样的困境：**无法获取网络文本的语义信息**。尽管近些年人工智能得到了长足的发展，在某些任务上取得超越人类的成绩，但离一台机器拥有一个两三岁小孩的智力的目标还有一段距离。这距离的背后很大一部分原因是机器缺少知识。**机器无法理解文本本身**，无法在看到文本后进行自主的推理和联想。**为了让机器能够理解文本背后的含义，我们需要对可描述的事物(实体)进行建模，填充它的属性，拓展它和其他事物的联系，即构建机器的先验知识**。机器具备来先验知识后，当它再次看到文本时，它就会主动的“联想”。这和人类在看到熟悉的事物，会做一些联想和推理是很类似的。

知识图谱首次在IT中的应用是在2012年5月17日，Google为了提升搜索引擎返回的答案质量和用户查询的效率，便发布知识图谱来作为辅助。这样搜索引擎能够洞察用户查询背后的语义信息，返回更为精准、结构化的信息，更大可能地满足用户的查询需求。在很多情况下，用户的搜索意图可能是模糊的。在之前的版本，我们在搜索某段文本时，只能得到包含这个文本的相关网页作为返回结果，然后不得不进入某些网页详细查找我们感兴趣的信息；现在，除了相关网页，搜索引擎还会返回一个“知识卡片”，包含了查询对象的基本信息和其相关的其他对象。那么我们不用再做多余的操作。在最短的时间内，我们获取了最为简洁，最为准确的信息。

Google知识图谱的宣传语“things not strings”给出了知识图谱的精髓，即不要无意义的字符串，而是获取字符串背后隐含的对象或事物。

以上只是知识图谱一部分的应用场景。目前知识图谱并没有一个标准的定义，但它的设计理念并不新，其背后的思想可以追溯到上个世纪五六十年代提出的语义网络，以图形化的形式通过点和边表达知识。除了语义网络，专家系统、万维网之父Tim Berners Lee于1998年提出的语义网和在2006年提出的关联数据都和知识图谱有着千丝万缕的关系，可以说它们是知识图谱前身。

目前，国内外多个研究机构建立了一些大型通用知识图谱。在国内,代表性的通用知识图谱包括搜狗知立方、百度知心、OpenKN、CN-DBpedia等；在国外，代表性的通用知识图谱包括WordNet、DBpedia、Freebase、YAGO、Probase、Knowledgevault等。近年来，在一些领域已经出现面向领域的知识图谱，包括电影领域的IMDB、生物医学领域的DrugBank、新闻领域的ECKG、学术领域的Acemap等。通用知识图谱和领域知识图谱的区别主要体现在知识建模与覆盖范围上：

1. 通用知识图谱面向通用领域，以**常识性知识**为主。其构建过程**高度自动化**，通常采用自底向上的方式来构建。其关联的知识大多数是**静态**的、客观的、明确的三元组事实性知识。一般以互联网开放数据为基础，再逐步扩大数据规模；
2. 领域知识图谱面向某一特定领域，以行业数据为主，其构建过程是**半自动化**的。通常采用自顶向下和自底向上两种方式相结合的方式来构建。其关联的知识包含静态知识和动态知识。

此外，从图谱构建的具体子过程来看，通用知识图谱和领域知识图谱还存在以下区别：

1. 从知识抽取角度来看，通用知识图谱注重**知识的广度**，覆盖粗粒度的知识。其在实体抽取层面，**关注更多的实体**，准确度不高；在关系抽取层面，多采用面向开放域的关系抽取。领域知识图谱注重**知识的深度**，覆盖细粒度的知识。其在实体抽取层面，关注具有特定行业意义的领域数据，准确度高；在关系抽取层面，多采用**预定义关系抽取**。
2. 从知识表示角度来看，通用知识图谱将知识表示成**多个互相关联的三元组**。例如，(实体1，关系，实体2)或(实体，属性，属性值)，各部分之间都有明确的层次结构。领域知识图谱除了将知识表示为多个互相关联的三元组之外，还需要对**专家经验知识、行业文本的语义信息**进行表示。
3. 从知识融合角度来看，通用知识图谱对知识抽取的质量有一定容忍度，需要通过知识融合来提升数据质量。**领域知识图谱从领域内部的结构化数据、半结构化数据、非结构化数据中抽取知识，并且有一定的人工审核校验机制来保证质量，需要通过知识融合来扩大数据层的规模。**
4. 从知识推理角度来看，由于通用知识图谱的知识覆盖范围较宽，深度较浅，从而导致图谱上的推理路径相对较短。而领域知识图谱的知识相对密集，这就导致**图谱上的推理路径相对较长**。当然也存在一些特殊情况，例如DBpedia具有丰富的推理规则，推理路径比某些只有少量推理规则的领域知识图谱长。另外，推理路径上的区别体现在上层本体和垂直本体的比较上。

通过上述通用知识图谱和领域知识图谱的比较，可以发现两者在构建过程中存在很多区别。但是，在实际的工程实践中，两者之间也存在着较强的联系。例如，构建领域知识图谱需要借鉴通用知识图谱的方法，引入通用知识库进行知识的融合。但是，全盘接收通用知识图谱中的数据会引入大量领域不相关的信息，影响领域知识利用的效果。因此，可以利用通用知识图谱的广度结合领域知识图谱的深度，形成更加完善的知识图谱。

本课题构建的方向是产业链知识图谱。目前市面上已经有类似的产品，如前瞻产业研究院的产品[https://x.qianzhan.com/datav/chanyelian/](https://x.qianzhan.com/datav/chanyelian/)，

他们将热点行业的产业链的相关企业大类和小类进行归类。例如大类是芯片，小类就是芯片的上中下游的各个环节，每个小类都显示了企业数量。产业在地图的分布也利用热力图进行显示。他们的整体思路就是从产业链入手，为热门行业建立了产业链图，然后找出产业链的大类和小类包含的企业。按照地理位置，绘制出产业分布热力图。这家公司对产业链有一定的研究，但是视野还是不够宽，产品内部缺少研究产业集群的部分。

相关专利大体的研究模式也是如此，根据企业的相关文本，提取出关键词，将产品归为产业链的某个环节(大类，小类)。相应的，企业也就可以被归为产业链的某个环节。这样，产业链图带有上、中和下游，多个环节。每个环节附有产品集合，企业集合。再根据企业的位置就可以知道某个区域某条产业链都有哪些企业和产品，它们都分布在产业链的哪些环节。这样就可以知道该区域、该产业链是否存在产业集群了。

从中我们可以发现，现有的产业知识图谱都着重突出了知识联系，但没有将各种产业、企业代入到关系之中，换言之，它们研究的是静态的产业知识图谱和静态的产业知识。而本课题研究的是将真实产品、真实企业、真实集群、真实产业链代入到产业图谱，这是一种动态的产业知识图谱。

# 主要研究内容与研究方案

## 语义网络

在知识表示中，**知识图谱**是一种知识库，其中的数据通过图结构的数据模型或拓扑整合而成。知识图谱通常被用来存储彼此之间具有相互联系的实体。其基本组合单位是<实体,关系,实体>三元组，和实体及其相关属性的属性值对，实体之间通过关系相互联结，构成网状的知识结构。

狭义的知识图谱特指一类知识表示，本质上是一种大规模语义网络，广义的知识图谱是大数据时代知识工程一系列技术的总称。

语义网络(Semantic Network)常常用作知识表示的一种形式。它其实是一种有向图；其中，顶点代表的是概念，而边则表示的是这些概念之间的语义关系。

![1.png](https://Adamisyoung.github.io/assets/pic/YangtzeRiverEconomicBelt(Basics)/1.png)

上图有几种关系：

- is-a关系。比如：猫是一种哺乳动物；
- part-of关系，比如：脊椎是哺乳动物的一部分；
- ......

语义网络能够以人的一种思维方式将事物之间的相互关系表示出来，将人的思维过程规则化成为一种知识构架，而且此种表示方法通过一定的处理过程就能成为计算机所能接受和理解的知识。

尽管语义网络容易理解，相关概念容易聚类，但它还是存在很大的缺憾：

- 语义网络高度定制化，需要用户自己定义节点和边的值，缺乏客观性，且没有标准的约束；
    
    RDF在节点和边的取值上做了约束，制定了统一标准，为多源数据的融合提供了便利。
    
- 难以融合多源数据；
- 无法区分概念节点(Class)和对象节点(Object)。
    
    这点比较棘手，简单举个例子。假如我们有两个语义网络A和B。在A中，熊是哺乳动物的一个实例。在B中，熊是哺乳动物的一个子类。前者是is-a关系，后者是SubClassOf关系。这种情况常有发生，**我们建模的角度不同，那么同一个事物的表示也可能不同**。如果我们不能用一种方法来区别两者，不仅会给我们带来理解上的困难，在进行融合的时候也会造成**数据冲突**。我们不能说A既是B的一个实例，又是B的一个子类。W3C制定的另外两个标准**RDFS/OWL**解决了这个问题。
    
- 无法对节点和边的标签进行定义。(Schema层)

简而言之，语义网络可以比较容易地让我们理解语义和语义关系。但是由于其缺少规范性和缺少节点的划分所以存在不少的问题。因此，W3C提出一系列技术栈对这些问题进行解决，其中OWL和RDF是最先出现的。这属于**语义网(Semantic Web)**的范畴。

![2.png](https://Adamisyoung.github.io/assets/pic/YangtzeRiverEconomicBelt(Basics)/2.png)

## 语义网和链接数据

参考：[https://zhuanlan.zhihu.com/p/31864048](https://zhuanlan.zhihu.com/p/31864048)

语义网和链接数据是万维网之父Tim Berners Lee分别在1998年和2006提出的。相对于语义网络，语义网和链接数据倾向于描述万维网中资源、数据之间的关系。本质上语义网、链接数据还有Web 3.0都是同一个概念，只是在不同的时间节点和环境中，它们各自描述的角度不同。**它们都是指W3C制定的用于描述和关联万维网数据的一系列技术标准，即语义网技术栈**。

**语义网**是一个更官方的名称，也是该领域学者使用得最多的一个术语，同时，也用于指代其相关的技术标准。在万维网诞生之初，网络上的内容只是人类可读，而计算机无法理解和处理。比如，我们浏览一个网页，我们能够轻松理解网页上面的内容，而计算机只知道这是一个网页。网页里面有图片，有链接，但是计算机并不知道图片是关于什么的，也不清楚链接指向的页面和当前页面有何关系。语义网正是为了使得网络上的数据变得机器可读而提出的一个通用框架。“Semantic”就是用更丰富的方式来表达数据背后的含义，让机器能够理解数据。“Web”则是希望这些数据相互链接，组成一个庞大的信息网络，正如互联网中相互链接的网页，只不过基本单位变为粒度更小的数据，如下图。

![3.png](https://Adamisyoung.github.io/assets/pic/YangtzeRiverEconomicBelt(Basics)/3.png)

**链接数据**起初是用于定义如何利用语义网技术在网上发布数据，其强调在不同的数据集间创建链接。Tim Berners Lee提出了发布数据的[四个原则](https://link.zhihu.com/?target=https%3A//www.w3.org/DesignIssues/LinkedData.html)，并根据数据集的开放程度将其划分为1到5星5个层次。链接数据也被当做是语义网技术一个更简洁，简单的描述。当它指语义网技术时，它更强调“Web”，弱化了“Semantic”的部分。对应到语义网技术栈，它倾向于使用RDF和SPARQL(RDF查询语言)技术，对于**Schema层的技术，RDFS或者OWL，则很少使用**。链接数据应该是最接近知识图谱的一个概念，从某种角度说，知识图谱是对链接数据这个概念的进一步包装。

关于知识图谱的知识表达，详细内容参考这篇文章([https://adamisyoung.github.io/notebook/knowledge graph/2022/02/26/Knowledge-Representation-in-Knowledge-Graph/](https://adamisyoung.github.io/notebook/knowledge%20graph/2022/02/26/Knowledge-Representation-in-Knowledge-Graph/))

目前，当前开放知识图谱的规模，涉及的领域以及知识图谱间的链接关系可以查看[开放链接数据项目](https://link.zhihu.com/?target=http%3A//linkeddata.org/)进展的[可视](https://link.zhihu.com/?target=http%3A//lod-cloud.net/)化程度。

![4.png](https://Adamisyoung.github.io/assets/pic/YangtzeRiverEconomicBelt(Basics)/4.png)

不过，链接数据更强调不同RDF数据集(知识图谱)的相互链接；知识图谱不一定要链接到外部的知识图谱(和企业内部数据通常也不会公开一个道理)，更强调有一个本体层来定义实体的类型和实体之间的关系。

## 知识图谱

现在我们用更正式一点的说法，知识图谱是由本体作为Schema层，和RDF数据模型兼容的结构化数据集。本体本身是个哲学名词，AI研究人员于上个世纪70年代引入计算机领域。Tom Gruber把本体定义为“概念和关系的形式化描述”，分别指实体的类层次和关系层次。在RDF实例中，我们用IRI唯一标志的节点都是某个类的一个实例，每一条边都表示一个关系。

以下是知识图谱的完全体模型(图中星号表示可以存在多个不同的属性或者关系)：

![5.png](https://Adamisyoung.github.io/assets/pic/YangtzeRiverEconomicBelt(Basics)/5.png)

- **实体**：实体是能够独立存在的、作为一切属性的基础和万物本原的东西。也就是说，实体是属性赖以存在的基础，并且必须是自在的，即独立的、不依附于其他东西而存在的；
- **概念**：概念又被称为类别。比方说，我们描述一类人时，他们有着相同的描述模板，这就构成了一个类或者一个概念；
- **值**：每个实体都有一定的属性值。属性值可以是常见的数值类型、日期类型或者文本类型。

关系我们也称为属性(Property)，根据是实体和实体之间的关系还是实体和数据值之间的关系分为对象属性(Object Property)和数据属性(Data Property)。前者如某个人的父亲是一个特定的人物实体，因此"父亲"可以认为是一条关系；后者就像人的出生日期、身高、体重等。

关系对于知识图谱上的多步遍历以及沿着语义关系的长程推理十分重要。而知识图谱上的推理操作一旦遇到一个属性，就意味着推理结束。比如，要想知道柏拉图的导师的出生时间，需要先在知识图谱中从“柏拉图”沿着"导师"关系找到"苏格拉底"，再沿着“苏格拉底”的“出生时间”属性找到最终答案，最后整个推理过程即宣告结束。

## 产业链与产业集群

### 理论

产业链是一个包含价值链、企业链、供需链和空间链四个维度的概念，这四个维度在相互对接的均衡过程中形成了产业链。

1. **价值链**：价值链的理论和供应链类似，但前者更关注企业内外价值增加的活动。波特教授将企业内外价值增加的活动分为基本活动和支持性活动，供应链涵盖了基本活动部分，而价值链列示了支持性活动在内的总价值。下图为价值链的一个例子；
    
    ![6.png](https://Adamisyoung.github.io/assets/pic/YangtzeRiverEconomicBelt(Basics)/6.png)
    
2. **企业链**：企业链是指由企业生命体通过物质、资金、技术等流动和相互作用形成的企业链条，组成企业链的企业彼此之间进行物质资金的交易实现价值的增值，又通过资金的反向流动相互联系。企业链是企业生命体与生态系统的中间层次。不同点上的企业对企业链的形成和稳定都有一定作用，企业的活力和优势决定了企业链的活力和优势，同时企业链也会对企业进行筛选，通过优胜劣汰实现企业和企业链的协同发展。企业链中的企业也通过不同渠道和这条企业链以外的企业进行合作，不同企业链之间是相互联系的网状结构。优势企业会形成核心节点，占据优势位置；
3. **供需链**：供需链是指产品生产和流通过程中所涉及的原材料供应商、生产商、分销商、零售商以及最终消费者等成员通过与上游、下游成员的连接组成的网络结构。即是由物料获取、物料加工、并将成品送到用户手中这一过程所涉及的企业和企业部门组成的一个网络。供应链可以分为内部供应链和外部供应链两类，即前者是企业内部产品生产和流通过程中的供需网络，后者是企业外部的供需网络；
    
    一般来说，构成供需链的基本要素包括：供应商、厂家、分销企业、零售企业和物流企业；供需链包括四个流程：物流、商流、信息流和资金流四个流程。
    
4. **空间链**：空间链是指同一种产业链条在不同地区间的分布。在全球范围内就有很多同种类型的产业链。对于同一种产业链来说，在不同地方有不同的分布。空间链中的对接主要是指产业链条的地域分布问题。产业链的分布分为全球、国家、地区三个层次，客观上要求产业链在三个层次之间相互协调。产业链上诸产业链环总是从空间上落脚到一定地域，在宏观经济视野中，链条基本是环环相扣而完整的，而从区域经济视角来看，链条未必就是完整的。

产业链的四个维度在宏观、中观和微观三个层次之间进行对接。对接的内容影响着产业链的延伸和扩展，对接内容越丰富，产业链就越长。如下表所示：

![7.png](https://Adamisyoung.github.io/assets/pic/YangtzeRiverEconomicBelt(Basics)/7.png)

### 实体关系概括

技术的发展和需求的进步引发供需链的变化，而企业链是产业链的有效载体，引发企业之间有效对接的因素就是**供需关系**。当企业链开始对接时，由于地理位置等多种因素，直接导致产业链的组织形式、空间布局、供需流动有所差异，这些差异会促使企业链之间不断竞争并推动产业链的不断演变。最后形成区域化的产业集群现象。

这几个链的对接关系如下：

- **供需链中的对接**：本质上说是消费需求和生产需求的对接。消费需求必然引发生产需求并导致消费需求和生产需求之间、各级生产需求之间、需求与供给能力之间的相互衔接；
- **空间链的对接**：产业链的地域分布。这种对接要求产业链在全球、国家、地区三个层次上相互对接。它可分为同一产业链内部节点之间、不同产业链之间和不同产业链之间部分环节的交叉对接；
- **企业链中的对接**：这种对接主要分为同一条产业链上企业之间的对接和不同产业链上企业之间的对接。前者实际上是分工和交易的问题，即企业的边界问题：后者则是在不同市场结构和政府产业政策影响下的企业横向协作与扩张问题。

综上，若是从小到大的概括，首先一个企业具有如下属性：

- 产品：面向需求方；
- 技术标准：源于技术硬实力，技术创新带来技术标准的创新；
- 生产资料：源于供应商；
- 辐射半径：产品的辐射范围，企业的影响力。

其次，企业和企业之间存在如下关系：

- 同一产业内的分工、交易和市场竞争，各企业在同一产业的市场份额等等；
- 不同产业间，由不同的市场结构和政府产业政策影响下的企业横向协作与扩张。

形成区域性质的产业集群后：

- 产业链的延伸和升级：产业转移，在全球产业链中占据上游位置。

以上可以很好的构建产业链知识图谱。

### 长江经济带

长江经济带是我国重大战略发展地区，覆盖上海、江苏、浙江、安徽、江西、湖北、湖南、重庆、四川、云南等11个省市，横跨中国东中西三大区域。

2014年9月，国务院印发《关于依托黄金水道推动长江经济带发展的指导意见》，部署将长江经济带建设成为具有全球影响力的内河经济带、东中西互动合作的协调发展带、沿海沿江沿边全面推进的对内对外开放带和生态文明建设的先行示范带。

长江是货运量位居全球内河第一的黄金水道，在区域发展总体格局中具有重要战略地位。依托黄金水道推动长江经济带发展，打造中国经济新支撑带，有利于挖掘中上游广阔腹地蕴含的巨大内需潜力，促进经济增长空间从沿海向沿江内陆拓展；有利于优化沿江产业结构和城镇化布局，推动中国经济提质增效升级；有利于形成上中下游优势互补、协作互动格局，缩小东中西部地区发展差距；有利于建设陆海双向对外开放新走廊，培育国际经济合作竞争新优势；有利于保护长江生态环境，引领全国生态文明建设。

我们可以预先定义出一条以长江为核心的完整的空间链，以此为脉络，通过对重点企业的实体识别，抽取出供需关系，确认资金流、物流等的流向，以此来系统的对长江上下游产业链进行信息抽取和分析。

## 研究方法

待解决的流程大致分为四步：

1. 产业链数据源；
2. 产业链知识抽取；
3. 产业链知识图谱；
4. 产业集群识别。

分为两种类型：

- 工程快捷型
    1. 产业链方面：
        - 利用国民经济行业分类体系，建立产业链的上中下游的各个环节的大小类(产业链本体)；
        - 用最简单的分类模型，对企业产品进行环节的归类；
        - 利用可视化技术展示产业链(各个环节组成的产业链，以及每个环节的企业和产品)。
    2. 集群方面：
        
        根据产业的大类，自动发现区域的企业集合(集群)，以及这些企业所占据的产业链的环节个数，个数越多，说明这些企业越可能构成了区域产业链，越可能是个区域集群。这些会有相关指标和计算方法来进行辅助。
        
- 学术缓慢型
    1. 产业链方面：
        
        在产业或企业的相关文本上，利用**命名实体识别**、**关系抽取**等知识抽取算法，抽取产业链知识，建立产业链知识图谱(产业链图谱)；
        
    2. 集群发现算法研究。

## 数据源

- 海关商品编码表提供了商品(产品)名录。所以我们能从中提取出**产品分类体系**，从而丰富产品库，打造产业链上中下游分类体系；
- 天眼查，企查查等能提供**企业名录、企业地理位置**，可以丰富企业库，也能根据企业的地理位置和企业的分类发现企业所属的集群；
- 需要企业的**产品描述**数据，这些数据可辅助产品分类，企业分类，从而进行产业链上大小类的区分；

简而言之，根据企业文本数据、海关商品编码表(产品名)和海关商品的大类和小类组成的分类体系(大类对应产业链，小类对应产业链的某个环节)、以及待研究的产品的产业链分类算法、企业的产业链分类算法，就可以得到产业链图，也能清晰算出某个区域的产业链，是否存在集群。

目前一大缺憾就是缺少企业贸易数据(企业之间的交易数据，这些数据存在于税务部门的发票数据上，银行部门的转账数据上，都是非公开数据)。海关有企业的进出口数据，但是也很难得。这样对研究贸易网络存在一些阻碍。但影响不大。