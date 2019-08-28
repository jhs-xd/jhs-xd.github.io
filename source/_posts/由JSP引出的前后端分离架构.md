---
title: 由JSP引出的前后端分离架构
copyright: true
categories: 
  - 钻牛角尖
tags: 
  - JSP
  - HTML+ajax

---
<blockquote class="blockquote-center">The man with a new idea is a crank until the idea succeeds.——Mark Twain
![](/myimages/8.png)<blockquote>

<!-- more -->

---

# 写在前面 #
今天写了一天项目申请书，心累。
说说昨天在学习JSP时想到的一个问题，以及由此引发的我一连串的百度搜索...


> 为什么要有JSP这么个东西？

---

# 有了Servlet为什么要有JSP #

刚接触的时候，老师由这个问题引出了JSP
Servlet和JSP都属于动态web资源，都是服务器端的技术，需要启动服务器后才会执行，那它俩有啥区别呢？为什么有了Servlet为什么要有JSP？

	Servlet是一种运行在服务器端的java小程序，它通常通过http协议接收和响应来自web客户端的请求。

	有人说JSP就是Servlet，我觉得有一定道理：服务器在读到JSP代码后，根据相应的业务逻辑，编译成相应的servlet程序，再由servlet输出到页面（输出的就是html），但Servlet可能更偏向于逻辑的实现，而JSP偏向于与HTML页面相关的操作
	
	Servlet程序中还可以编写前端HTML页面：response.getWriter("HTML代码")
	但是用Servlet生成HTML页面有不可避免的缺点：
	1. 代码量大
	2. 每一个servlet程序都需要配置
	3. 要求开发人员十分了解java，不利于前后端分离，不便于页面的调试、维护

	老师由此引出了JSP，但后来我发现JSP并没有完全解决上面存在的问题啊
	1. JSP文件不需要像Servlet一样在服务器端进行配置web.xml，就能在浏览器端访问。这算解决了第二个问题
	2. 可是，JSP可以用来编写HTML+java+JSP代码，这些代码那么乱的结合在一起，也没有解决前后端分离的问题，相反我觉得问题更严重了

到这儿问题又来了：为什么一定要做到前后端分离，有什么好处？有没有必要？

---

# 前后端分离的架构 #

> 这一部分摘自[LIDADA博客](http://lidada.org/front-end-development-process-and-front-end-separation-mode/)

## 传统开发模式 ##

这种方式的特点是利用后端语言提供的模板引擎生成html页面，再经服务器返回到客户端浏览器中。而浏览器就只需要解析这些代码就行了。

常见的开发方式有：

PHP语言的Smarty模板引擎与Thinkphp框架
Java语言的Freemarker模板引擎与Jsp页面

## Ajax请求 ##
Ajax是前后端分离的推进者，使用Ajax后，网页可以实现局部刷新，不需要再依靠刷新页面对网页中的内容进行更新了。
同时后台仅需要给前台暴露出前台需要的各种API接口，并对前台提供的接口数据进行增删改查就可以了。
这无疑也剩下了很多工作量，也为前端的下一步发展打下了基础。

## 前端构建与请求 ##
传统开发模式中，前端的所有文件都放在了后台的server中。后端的项目通常都会有自己的server，除了php以外。前端构建的话，前端项目也要搭建一个server，然后把前端的项目放到apache或者nginx中。或者利用nodeJs工具集。

现在前后端分离开了，当然也涉及到请求的问题。这时我们只需要告诉后端服务我们需要的数据就可以了。但这样会产生一个问题，Ajax跨域问题。在这里我们不能用常用的jsonp或者iframe信使等去解决问题，因为我们还有POST请求。

所以HTTP Proxy类工具就可以用到了，比如我再BrowserSync加入中间件判断每一个请求，如果是/api为前缀的就会被代理到API Server端，API server端接收到数据后再返回给BrowserSync，然后BrowserSync再返回给浏览器。

生产环境可以前后端分开部署，只需要在前端的server中写好转发规则就可以了，apache和nginx都支持的。

## 总结 ##
1. 前后端分离的优势：

		前端静态资源与后台api分流，互不影响
		前后台同步开发，减少沟通成本
		方便开发调试，不影响工作进度
		易于维护扩展

2. 前后端分离缺点：
		前端负载增加
		不利于搜索引擎优化

---

# 一些理解 #
1. 前端使用HTML+Ajax，后端使用Java Servlet，这样完全可以做到前后端分离。
前端哪天换成了移动App或者桌面App，后端程序可重用、无需重新开发；而后端服务如果需要换成.Net或PHP，前端也可完全重用，前后端做到100%解耦，这就是前后端分离的架构思想。

2. 大型的项目肯定是HTML+AJAX，HTML只要浏览器便能解析了，像JSP还要服务器解析编译。
jsp这种只能在自己的小项目或者后台系统，像平台级的项目都是HTML+AJAX，这样前后端分离，前后端可以同时由不同的分工开发。

3. 如果你注重安全和浏览器响应效率，可以用html，毕竟他是静态网页，加上ajax相当于给他动态行为
如果你注重开发效率，可以用jsp， 毕竟他封装的比较多,用起来肯定爽的多

4. jsp是页面先加载数据，ajax是页面后加载数据，这个对搜索引擎的影响很大，若你是做系统之类的项目不需要搜索引擎收录，那区别不大，也就点点效率问题。
可是要考虑收录的话你用ajax的方式后加载数据会让搜索引擎获取不到这些内容，你的页面能呈现的只是那些静态的内容，无论你ajax获取的数据有多不同，有多少个页面，对于搜索引擎来说那就是一个页面，因为呈现的内容是一样的

5. 总结

HTML+ajax

 - 有利于前后端分离，便于后期维护，便于前端代码的移植
 - HTML浏览器响应效率高，还可以将动静态资源分离，HTML部署在nginx上，效率更高
 - 安全

JSP

 - 封装较多，开发效率高
 - 如果需要考虑搜索引擎收录，由于jsp是页面先加载数据，ajax是页面后加载数据，那么用ajax的方式后加载数据会让搜索引擎获取不到这些内容，你的页面能呈现的只是那些静态的内容

---

# 比较好的相关文章 #
先收着，指不定哪天就醍醐灌顶了

[Web研发模式演变](https://github.com/lifesinger/blog/issues/184)


[浅谈架构之路——前后端分离模式](https://www.cnblogs.com/shanrengo/p/6397734.html)

从经典的JSP+Servlet+JavaBean的MVC时代，到SSM（Spring + SpringMVC + Mybatis）和SSH（Spring + Struts + Hibernate）的Java 框架时代，再到前端框架（KnockoutJS、AngularJS、vueJS、ReactJS）为主的MVC时代，然后是Nodejs引领的全栈时代，技术和架构一直都在进步。