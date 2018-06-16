---
layout: post
title: 博客模板
category: 技术
tags: Blog template,博文模板
description: 博文模板
#published: true    #如果不想发布某一篇文章，你可以设置此项为false.
---
# Markdown常用规则
## Markdown语言来龙去脉  
Markdown 是一种轻量级标记语言，创始人为约翰·格鲁伯（John Gruber）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档”。这种语言吸收了很多在电子邮件中已有的纯文本标记的特性。

Markdown 的目标是实现「易读易写」。可读性，无论如何，都是最重要的。一份使用 Markdown 格式撰写的文件应该可以直接以纯文本发布，并且看起来不会像是由许多标签或是格式指令所构成。Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括Setext、atx、Textile、reStructuredText、Grutatext 和 EtText，而最大灵感来源其实是纯文本电子邮件的格式。总之， Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然。比如：在文字两旁加上星号，看起来就像*强调*。Markdown 的列表看起来，嗯，就是列表。Markdown 的区块引用看起来就真的像是引用一段文字，就像你曾在电子邮件中见过的那样。

Markdown 语法的目标是：成为一种适用于网络的书写语言。Markdown 不是想要取代 HTML，甚至也没有要和它相近，它的语法种类很少，只对应 HTML 标记的一小部分。Markdown 的构想不是要使得 HTML 文档更容易书写。在我看来， HTML 已经很容易写了。Markdown 的理念是，能让文档更容易读、写和随意改。HTML 是一种发布的格式，Markdown 是一种书写的格式。就这样，Markdown 的格式语法只涵盖纯文本可以涵盖的范围。

正是因为Markdown的这些特点，而且功能比纯文本更强，因此有很多人用它写博客。世界上最流行的博客平台WordPress大型CMS如joomla、drupal都能很好的支持Markdown。

---
##下面介绍Markdown常用语法
---
### 插入标题
 标题使用不同数量的"#"来标识是什么层级，可以对应于HTML里面的H1-H6，下面是示例代码和效果：
    ![Mou icon](/assets/img/20141121163201450.png)
 “========”风格的也可以，但是我不喜欢，赶不上"#"的好用
---
### 插入图片测试
我们可以使用下面的语法，添加一个图片
    ####![Alt text](/path/to/img.jpg)
    详细叙述如下：
    一个惊叹号 !
    接着一个方括号，里面放上图片的替代文字
    接着一个普通括号，里面放上图片的网址

  下面是一个示例：
     ![Mou icon](/assets/img/20141121163821625.png)

---
### 插入附件测试
   如果我们想在文章中添加代码，我们有两种方式
   第一种方式是使用反引号(esc键下面的按钮)将代码包裹起来
   下面是一个示例代码
---

### 插入代码测试
This is a test text

def xxx:
  a = 1
---
### 插入分割线
（8）分割线
    如果我们想用分割线对内容进行分割，我们可以在单独一行里输入3个或以上的短横线、星号或者下划线实现。短横线和星号之间可以输入任意空格。以下每一行都产生一条水平分割线。
    ![line](/assets/img/20141121171114184.png)
---
### 插入列标记
    如果我们的内容需要进行标记，那么我们可以使用下面的方式
  ![line](/assets/img/20141121171601583.png)
---
### 换行
  如果我们想把一行文本进行换行，我们可以在需要换行的地方输入至少两个空格，然后回车即可，注意，如果不回车，是没有效果的，就像下面这样
  ![line](/assets/img/20141121170040687.png)
---
### 引用
如果我们在文章中引用了资料，那么我们可以通过一个右尖括号">"来表示这是一段引用内容。我们可以在开头加一个，也可以在每一行的前面都加一个。我们还可以在引用里面嵌套其他的引用，下面是一个示例：
  ![line](/assets/img/20141121170507567.png)
---
### 强调
我们可以使用下面的方式给我们的文本添加强调的效果

*强调* 或者 _强调_  (示例：斜体)
**加重强调** 或者 __加重强调__ (示例：粗体)
***特别强调*** 或者 ___特别强调___ (示例：粗斜体)

    下面是一个示例：
  ![line](/assets/img/20141121164141381.png)
---
### 插入代码
如果我们想在文章中添加代码，我们有两种方式
   第一种方式是使用反引号(esc键下面的按钮)将代码包裹起来
   下面是一个示例代码
  ![line](/assets/img/20141121165433515.png)

`This is a code by backtick`  

测试一下:  
```python
import configparser
import os
import sys
import pprint
BASIC_SECTION ='Basic_Config'
current_path = os.getcwd()
if 'config.ini' in os.listdir(current_path):
    config_path = os.path.join(current_path,'config.ini')
    cf = configparser.ConfigParser()
    cf.read(config_path)
    MONGODB_IP = cf.get(BASIC_SECTION,'mongodb_ip')
    MONGODB_PORT = cf.get(BASIC_SECTION,'mongodb_port')
    MONGODB_USER = cf.get(BASIC_SECTION,'mongodb_user')
    MONGODB_PWD = cf.get(BASIC_SECTION,'mongodb_pwd')
    MONGODB_DB = cf.get(BASIC_SECTION,'mongodb_db')
    MONGODB_SYS_DB = cf.get(BASIC_SECTION,'mongodb_system_db')
    MONGODB_TENANT_DB = cf.get(BASIC_SECTION,'modgodb_tenant_db')
    USER=cf.get(BASIC_SECTION,'user')
    PASSWORD=cf.get(BASIC_SECTION,'pwd')
    DOMAIN = cf.get(BASIC_SECTION,'domain')
    TENANT = cf.get(BASIC_SECTION,'tenant')
    HOST_URL = cf.get(BASIC_SECTION,'host_url')
    MONGODB_SSL = cf.get(BASIC_SECTION,"mongodb_ssl")

else:
    print('Current path does not config.ini file')
    sys.exit()

def log(result):
    if isinstance(result,dict) and result.get('statusCode') == 0:
        pprint.pprint(result)
    elif isinstance(result,str):
        pprint.pprint("======="+ result+"==========")
    elif isinstance(result,dict):
        pprint.pprint(result)
    else:
        pprint.pprint('execute API Failed:%s'%result)

```
---
### 字体
有人会问，这货能改变颜色麽？额，貌似Markdown基本语法不支持任意更改颜色的功能。Markdown的理念是——注重文本本身，特效神马的交给CSS搞定。当然了，既然这货支持HTML嵌套，我们可以利用HTML标记实现更改颜色的需求。

更改代码颜色代码如下：  
Default Color  
<font color='red'>Red Color</font>  
<font color='blue'>Blue Color</font>
<font color='green'>Green Color</font>  
<font color='yellow'>Yellow Color</font>
<font color='pink'>Pink Color</font>
<font color='purple'>Purple Color</font>
<font color='orange'>Orange Color</font>

既然可以换颜色了，那换字号、字体也可以用HTML轻松实现了。以下代码可显示换字号、字体功能。

<font size='-2'>Small Size</font>
Normal Size
<font size='+2'>Big Size</font>
<font size='+2' face='Times'>Time New Roman</font>

其输出如下：  
Small Size  
Normal Size  
Big Size  
Time New Roman

话说嵌入这些格式化字体的HTML标签后，整个文本显得臃肿了不少。这有悖于Markdown注重文本内容、精简格式化标签的理念，因此文本特效神马的，能不用就尽量不用，这些统统扔给CSS去解决，我们只关注文本内容。

---
