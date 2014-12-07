#什么是URL？

###简介
当你想去某人家里的时候，你要知道他家的地址。如果你想给你朋友打电话，你要知道你朋友的电话号码。没有这些信息，你想去别人家或者给人打电话都是不可能的。换句话说，如果你有别人的住址或者电话号码，你能立刻知道谁是谁，因为地址和电话号码都有其唯一性。

在互联网上寻找和访问服务器也是同样的一个概念。当你想上Facebook的游戏站的时候，你会打开浏览器然后输入[http://www.facebook.com/games](http://www.facebook.com/games)。web浏览器向这个地址发送一个HTTP请求，然后拿回这个地址的响应结果。你刚刚输入的那个地址,[http://www.facebook.com/games](http://www.facebook.com/games) 就是所谓的统一资源定位符(Uniform Resource Locator)或URL。URL就像是你跟你朋友沟通所需要的电话号码或者地址一样的存在。URL应该是统一资源标识符(Uniform Resource Identifier)或URI这个指定资源所在位置的概念里使用频率最高的部分了。本章我们讨论什么是URL及其组成部分，和对你这个web开发者来说URL意味着什么。

###URL组成部分
当你看到一个URL，比如```http://www.example.com/home```, 其实是由几个组件构成的。我们可以把这个URL分解成3部分：

* ```http```：通常被称为URL模式(scheme).总是出现在冒号和两个斜杠之前，作用是告诉web客户端怎样去访问一个资源。在本例中，它告诉web客户端使用超文本传输协议也就是HTTP去发起一个请求。常见的URL模式还有```ftp```, ```mailto```和```git```。
* ```www.example.com```：URL的第二个部分，就是资源路径或主机(host)。它告诉客户端，资源的确切位置。
* ```/home/```：URL的第三个部分就是URL路径。它代表了客户端正在请求什么样的本地资源(对于服务器来说)。
![http_components](http://d186loudes4jlv.cloudfront.net/http/images/url_components.png)

有时候，这个路径指向了一个主机上特定的资源。比如，```www.example.com/home/index.html```指向了example.com服务器上的一个HTML文件。

另外，URL可以包含一个主机用来监听HTTP请求的端口的端口号。一个```http://localhost:3000/profile```这样的URL，通过3000端口去监听HTTP请求。web客户端用来监听HTTP请求的默认端口号是80，尽管并不是总指定这个端口号，但它默认是每一个URL的一部分。**除非指定了其他的端口号代替，不然端口号80会被默认用于正常的HTTP请求**。

###查询字符串/参数
一个查询字符串或者参数是URL的一部分并且通常都包含一些要发往至服务器的各种类型的数据。一个简单的带查询字符串的URL长这样：
```ruby
http://www.example.com?search=ruby&results=10
```
让我们拆开来看看：

|查询字符串组件   | 描述             |
|:------------- |:--------------- |
|?              |这是个保留字，标识着查询字符串的开始 |
|search=ruby    |这是一个参数的键/值对儿        |
|&              |这是个保留字，需要给查询字符串添加参数时使用|
|rasult=10      |这也是一个参数的键/值对儿      |

现在我们再来看一个例子。假设我们有下面这个URL:

```ruby
http://www.phoneshop.com?product=iphone&size=32gb&color=white
```

![sample_url](http://d186loudes4jlv.cloudfront.net/http/images/query_string_components.png)

在上面这个例子里，键/值对儿```product=iphone```, ```size=32gb```, ```color=white``` 通过URL传给了服务器。这个请求告诉```www.phoneshop.com```的服务器，把要请求的资源条件限制在```产品iphone```，```大小32gb```和```颜色白色```。服务器怎么样使用这些参数取决于服务端的应用的处理逻辑。

另一个经常见到查询字符串的情况是当你在搜索引擎上搜索东西的时候。**因为查询字符串是通过URL传递的，他们仅使用HTTP的GET请求**。在本书后面的章节里我们会讨论不同的HTTP请求，但是现在你所需要知道的是，当你不论什么时候在浏览器的地址栏里输入网址进行浏览的时候，你就是在发起HTTP的GET请求。大部分超链接都是HTTP的GET请求，偶尔会有一些例外。

![get_request](http://d186loudes4jlv.cloudfront.net/http/images/query_strings.jpg)

使用查询字符串向服务器传递附加信息是个很棒的方法，但是对于查询字符串的使用，以下是一些限制：

* 查询字符串有最大长度。所以，如果你大量的数据需要传输，还是不要用查询字符串的好。
* 查询字符串中使用的键/值对儿是显示在URL上的。所以，不推荐用查询字符串传输敏感信息比如用户名或密码。
* 查询字符串中无法使用空格和特殊字符比如```&```。它们必须用URL编码代替，我们接下来会讨论这个。

###URL编码(URL Encoding)
URL在设计的时候就默认只接受ASCII码。因此，不安全的或者不是ASCII码的字符就要进行转义或者编码来适应这个格式。URL编码的原理是将不符合格式的字符替换成```%```开头后面跟着两个十六进制数字代表的ASCII码的一串字符。

下面是一些常见的URL编码和实例URL：

| 字符  | ASCII码  | URL                                                   |
| :---- | :-- | :-------------                                            |
|space  | 020 | http://www.thedesignshop.com/shops/tommy%020hilfiger.html |
|!      | 041 | http://www.thedesignshop.com/moredesigns%041.html         |
|+      | 053 | http://www.thedesignshop.com/shops/spencer%053.html       |

符合下列条件的字符都要进行编码处理：

1. 没有对应的ASCII码。
2. 字符的使用是不安全的。比如```%```就是不安全的，因为它经常用于对其它字符进行转义。
3. 字符是有特殊用途的URL模式保留字。有些字符用于保留字是有着特殊的意义；它们在URL中的存在具有特殊用途。比如```/```, ```?```, ```:```,和```&```, 都是需要进行编码的保留字。

比如```&```被保留用于查询字符串的分隔符。```:```也被保留用于分隔主机/端口号和用户名/密码。

那么什么样的字符能在URL里安全地使用呢？只有字母表里的和```$-_.+!'(),"```这些字符可以。如果保留字符被用于它的特殊用途，那么是可以不用编码的，不然也是必须要编码的。

###小结
本章，我们讨论了URL和什么是URL。我们也认识了URL的组件并以对URL编码的探索结尾。在准备工作章节之后，我们会稍微深入一点，一起去看看请求和响应，还有它们的构成。