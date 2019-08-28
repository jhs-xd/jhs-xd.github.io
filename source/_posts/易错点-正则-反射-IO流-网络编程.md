---
title: 易错点-正则+反射+IO流+网络编程
copyright: true
categories: 
  - 我的错题库
  - 牛客错题整理

---
<div align="center">**正则；反射；IO流；网络编程和JavaWeb**</div>
![](/myimages/5.jpg)

<!--more-->

---

# 正则 #

---

# 反射 #
	1 反射破坏代码的封装性，破坏原有的访问修饰符访问限制

---

# IO流 #
	1 一个文件中的数据要在控制台上显示，首先要把这个文件读进来，然后再输出，所以首先要建立文件输入流FileInputStream，然后再建立标准输出流 System.out

	2 File类
	  （1）File类是Java中对文件进行读写操作的基本类（×）
	  （2）File类是对文件整体或者文件属性操作的类，例如创建文件、删除文件、查看文件是否存在等功能。
	  File类不能操作文件内容，文件内容是用IO流操作的。
	  （3）不论是文本文件还是二进制文件，在计算机中都是以二进制形式存储的，所以都当做二进制文件读取

	3 使用ObjectOutputStream和ObjectInputStream可以将对象进行传输

	4 哪个类可用于处理Unicode?
	  答：InputStreamReader可以指定字符编码格式

---

# 网络编程/JavaWeb #
	1 Web工程的目录结构
      /WEB-INF/web.xml 是部署描述文件，用来初始化配置信息
      /WEB-INF/classes 用来放置应用程序用到的自定义类(.class)，必须包括包(package)结构
      /WEB-INF/lib 用来放置应用程序用到的JAR文件

	2 Socket通信编程
	  客户端通过new Socket()方法创建通信的Socket对象
	  服务器端通过new ServerSocket()创建TCP连接对象 accept接纳客户端请求

	3 connect()属于客户端，将此套接字连接到服务器

	4 URL一般有四部分组成： <协议>://<主机>:<端口>/<路径> 
	  <主机>是指主机在因特网上的域名。
	  http协议的默认<端口>为80（可以省略）

	5 判断：service()方法处理客户机发出的所有请求   （√）
	  解析：service()是servlet真正处理客户端传过来的请求的方法，由web容器调用，根据HTTP请求方法（GET、POST等），将请求分发到doGet、doPost等方法

	6 service()方法是接收请求，返回响应的方法
	  每次请求都执行一次，该方法被HttpServlet封装为doGet和doPost方法
	  service()是在javax.servlet.Servlet接口中定义的
	  doGet/doPost是在javax.servlet.http.HttpServlet中实现的

	7 servlet和CGI
	  （1）servlet处于服务器进程中，它通过多线程方式运行其他service方法
	  （2）CGI对每个请求都产生新的进程，服务完成后销毁
	  （3）CGI不可移植。为某一特定平台编写的CGI应用只能运行于这一运行环境中

	8 ServletContext、ServletConfig
	  （1）ServletContext中可以存放共享数据。ServletContext对象是真正的一个全局对象，凡是web容器中的Servlet都可以访问
	  （2）ServletConfig对象：用于封装servlet的配置信息（初始化参数），ServletConfig接口默认是通过GenericServlet实现的

	9 判断：servlet在多线程下使用了同步机制，因此，在并发编程下servlet是线程安全的  （×）

	10 2EE中，使用Servlet过滤器，需要在web.xml中配置什么元素？
	  答： Servlet过滤器的配置包括两部分：
      第一部分是过滤器在Web应用中的定义，由<filter>元素表示，包括<filter-name>和<filter-class>两个必需的子元素
      第二部分是过滤器映射的定义，由<filter-mapping>元素表示,可以将一个过滤器映射到一个或者多个Servlet或JSP文件，
	  也可以采用url-pattern将过滤器映射到任意特征的URL

	11 forward：服务器获取跳转页面内容传给用户，用户地址栏不变
	  redirect：是服务器向用户发送转向的地址，redirect后地址栏变成新的地址

	12 取http请求中的cookie值的方法：request.getHeader、request.getCookies

	13 HttpServletResponse功能：设置http头标，设置cookie，设置返回数据类型，输出返回数据
	  HttpServletRequest功能：读取路径信息...


	14 getParameter()是获取POST/GET传递的参数值
	  getInitParameter获取Tomcat的server.xml中设置Context的初始化参数
	  getAttribute()是获取对象容器中的数据值
	  getRequestDispatcher是请求转发

	15 有四种方法可以实现会话跟踪技术：URL重写、隐藏表单域、Cookie、Session

	16 WebServer
	  Webservice是跨平台，跨语言的远程调用技术;
	  它的通信机制实质就是xml数据交换;
	  它采用了soap协议（简单对象协议）进行通信

	17 JSP
	  （1）Jsp只会在客户端第一次发请求的时候被编译，之后的请求不会再编译，同时tomcat能自动检测jsp变更与否，变更则再进行编译。
	  （2）第一次编译并初始化时调用：init()；销毁调用：destroy()。在整个jsp生命周期中均只调用一次

	18 JSP分页代码中先取总记录数，得到总页数，最后显示本页的数据

	19 JSP内置对象
     request对象、response对象、session对象、out对象、 page对象、application对象、exception对象、 pageContext对象、config对象

	20 JSP 四大作用域：page (作用范围最小)、request、session、application（作用范围最大）
	  （1）存储在application对象中的属性可以被同一个WEB应用程序中的所有Servlet和JSP页面访问。（属性作用范围最大）
	  （2）存储在session对象中的属性可以被属于同一个会话（浏览器打开直到关闭称为一次会话，且在此期间会话不失效）的所有Servlet和JSP页面访问。
	  （3）存储在request对象中的属性可以被属于同一个请求的所有Servlet和JSP页面访问（在有转发的情况下可以跨页面获取属性值）
	  例如使用PageContext.forward和PageContext.include方法连接起来的多个Servlet和JSP页面。
	  （4）存储在pageContext对象中的属性仅可以被当前JSP页面的当前响应过程中调用的各个组件访问
	  例如，正在响应当前请求的JSP页面和它调用的各个自定义标签类

	21 JSP中静态include和动态include：
	  （1）动态 INCLUDE 用 jsp:include 动作实现 <jsp:include page="included.jsp" flush="true" /> 
	  它总是会检查所含文件中的变化 , 适合用于包含动态页面 , 并且可以带参数。各个文件分别先编译，然后组合成一个文件。
	  （2）静态 INCLUDE 用 include 伪码实现 , 但不会检查所含文件的变化 , 适用于包含静态页面 <%@ include file="included.htm" %>
	  先将文件的代码被原封不动地加入到了主页面从而合成一个文件，然后再进行编译，此时不允许有相同的变量。
	（3）区别：
	  include两种用法主要有两个方面的不同：
	  1）执行时间上:
    <%@ include file="relativeURI"%> 是在翻译阶段执行
    <jsp:include page="relativeURI" flush="true" /> 在请求处理阶段执行
	  2）引入内容的不同:
    <%@ include file="relativeURI"%>引入静态文本 (html,jsp), 在 JSP 页面被转化成 servlet 之前和它融和到一起
    <jsp:include page="relativeURI" flush="true" /> 引入执行页面或 servlet 所生成的应答文本



