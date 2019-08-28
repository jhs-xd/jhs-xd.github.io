---
title: Hexo&GitPages建站历程
copyright: true
categories: hello hexo
tags: hexo

---
<blockquote class="blockquote-center">Let's make a difference.![](/myimages/2.jpg)</blockquote>

<!--more-->

> 毛主席说了：前途是光明的，道路是曲折的。

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=320 height=86 src="//music.163.com/outchain/player?type=2&id=27845535&auto=1&height=66"></iframe>

# 写在前面 #
  这是我开始搭建个人博客的第三天，有了自己的一亩三分地，很开心。现在看来搭建个人博客并不是最初想的那么难，但即使按照教程来做，还是会遇到很多问题。接下来分享我使用hexo+gitpages搭建博客的过程以及一些你可能也卡壳的地方，希望能帮助到你。
  我会分为三部分介绍，第一部分搭建起博客的雏形，第二部分做一些个性化的配置（这个真的好玩，但也耗费时间），第三部分是我遇到的几个问题，避免你犯同样的错误浪费时间。

---
# 一 上线你的博客 #

## 获得个人网站域名 ##
在[阿里云](https://wanwang.aliyun.com/)购买属于你的域名

## GitHub创建个人仓库 ##
登录注册[GitHub](https://github.com/)，点击GitHub中的New repository创建新仓库，仓库名应该为：用户名.github.io

## 安装Git ##
Git是开源的分布式版本控制系统，用于敏捷高效地处理项目。我们网站在本地搭建好了，需要使用Git同步到GitHub上。

1. 下载安装[Git](https://git-scm.com/download/win)，在菜单里搜索Git Bash，设置user.name和user.email配置信息：
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"

2. 生成ssh密钥文件：
ssh-keygen -t rsa -C "你的GitHub注册邮箱"
然后直接三个回车即可，默认不需要设置密码
然后找到生成的.ssh的文件夹中的id_rsa.pub密钥，将内容全部复制
在github中找到Settings，新建new SSH Key
在Git Bash中检测GitHub公钥设置是否成功，输入ssh git@github.com

## 安装Node.js ##
Hexo基于Node.js，下载安装[Node.js](https://nodejs.org/en/download/)，检测Node.js是否安装成功，在命令行中输入 node -v，检测npm是否安装成功，在命令行中输入npm -v

到这了，安装Hexo的环境已经全部搭建完成！！！

## 安装Hexo ##
1. 在本地创建Blog文件夹，然后Git Bash Here
2. npm install -g hexo-cli安装Hexo
3. 初始化Blog hexo init
4. 新建博客并本地预览，检测网站本地是否能打开

		hexo new test_my_site
		hexo g
		hexo s
	浏览器输入 localhost:4000

## 推送网站 ##
将hexo创建的博客相关文件推送到github上
1. 将Hexo与GitHub关联
打开站点的配置文件_config.yml，翻到最后修改为：（注意：每一个冒号后面都有一个空格）

		deploy: 
		type: git
		repo: 这里填入你之前在GitHub上创建仓库的完整路径，记得加上 .git
		branch: master
2. 安装Git部署插件，输入命令：npm install hexo-deployer-git --save
3. 这时，我们分别输入三条命令：

		hexo clean 
		hexo g 
		hexo d
完成后，打开浏览器，在地址栏输入你的放置个人网站的仓库路径，即git用户名.github.io，你就会发现你的博客已经上线了，可以在网络上被访问了！！！

## 绑定域名 ##
1. 登录到阿里云，进入管理控制台的域名列表，找到你的个性化域名，进入解析
2. 添加CNAME解析： CNAME的记录值是：你的用户名.github.io
3. 登录GitHub，进入之前创建的仓库，点击settings，设置Custom domain，输入你的域名, save
4. 进入本地博客文件夹 ，进入blog/source目录下，创建一个没有后缀的文件命名为CNAME，输入你的域名，保存
5. git bash依次输入：hexo clean      hexo g       hexo d
6. 打开浏览器在地址栏输入你的个性化域名将会直接进入你自己搭建的网站！！！

---
# 二 个性化设置 #

## 更换主题 ##
1. 下载next主题：

		git clone https://github.com/iissnan/hexo-theme-next themes/next
2. 修改站点配置文件_config.yml中的theme: next
3. 在主题配置文件_config.yml中的Scheme Settings，可以选择next主题的样式

## 设置头像 ##
在站点 _config.yml 中添加 avatar: http://.... # 头像的URL（可以在markdownpad2中Ctrl+G生成）



## 添加网易云音乐模块 ##
1. 修改blog\themes\next\layout\_macro的sidebar.swig文件，添加网易云音乐网页版中音乐的外链代码
2. 设置音乐是否自动播放：修改代码中的auto为0/1即可

## 设置背景 ##
1. 把你挑选的背景图片命名为：background.jpg，放在blog\themes\next\source\images里，在blog\themes\next\source\css\_custom文件的custom.styl首部添加：

		body {background:url(/images/background.jpg);background-attachment: fixed;}
background-attachment: fixed;是固定背景图片。
2. 这是设置一张静态图片作为背景，其实Next主题自带有动态的背景效果，修改主题配置文件中的canvas_nest: false为canvas_nest: true即可

## 增加侧栏条目 ##
1. 修改主题配置文件_config.yml里的Menu Settings中的menu和menu_icons两个地方。
2. 其中menu里是配置菜单项对应的页面位置（如：/love），menu_icons对应菜单项的图标，这里的图标是来自于Font Awesome ，所以你需要在Font Awesome网站上找到你需要的icon，然后把该icon的名字写在menu_icons对应菜单名后面，注意冒号有一个英文输入状态的空格。
3. 设置好后，在命令行里输入：hexo new page "你所要增加的菜单项名称（要和你在menu中的填写要匹配）"
4. 新建的页面在博客根目录下的source文件里，这时你就可以对新建的页面自定义设计。

## 隐藏底部的poweredby ##
打开themes/next/layout/_partials/footer.swig,使用<!--   -->隐藏之间的代码即可

		<div class="powered-by"></div> <div class="theme-info"></div>

## 在每篇博文下面添加版权信息 ##
1. 详见
https://thief.one/2017/03/03/Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B/
2. 如果要在该博文下面增加版权信息的显示，需要在 Markdown 中增加copyright: true的设置

## 主页文章添加阴影效果 ##
打开\themes\next\source\css_custom\custom.styl,向里面加入：

		// 主页文章添加阴影效果
		.post {
		margin-top: 60px;
		margin-bottom: 60px;
		padding: 25px;
		-webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
		-moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
	 }

## 修改文章底部的那个带#号的标签 ##
修改模板/themes/next/layout/_macro/post.swig，搜索rel=”tag”>#，将 # 换成
 ![](/myimages/2.1.png)

## 将链接文本设置为蓝色 ##

		.post-body p a {
		color: #0593d3;
		border-bottom: none;
		&:hover {
		color: #0477ab;
		text-decoration: underline;
		}
		}

## 在右上角或者左上角实现fork me on github ##
修改 themes/next/layout/_layout.swig文件，放在
 ![](/myimages/2.2.png)
的下面，并把href改为你的github地址

## 添加来必力评论系统 ##
1. 注册L[ivere](https://livere.com/)，获取livere_uid，参考教程https://blog.smoker.cc/web/add-comments-livere-for-hexo-theme-next.html
2. 在主题 _config.yml 文件中添加如下配置： livere_uid= "你的来必力UID"

---
# 三 卡住我的地方 #

## 域名申请 ##
申请购买到属于你的域名后，一定要先实名认证，上传你的身份证正面照片即可（认证过程大约一两个小时）
阿里云注册的域名不认证用不了，我当时已经在本地部署好了博客网站的雏形（也就是hexo s后，浏览器打开localhost:4000），而且设置好域名CNAME解析到github用户名.github.io，但是在浏览器输入域名就404找不到网页。原因就是你的域名还不能用啊。

## 推送网站 ##
把hexo生成的博客部署/推送在github上这一步，修改站点配置文件_config.yml时要注意：

	deploy: 
	  type: git
	  repo: 
	  branch: master
这些代码每一个冒号后面都有一个空格，我浪费了大半天时间在同学的帮助下才找这个问题

## 添加来必力评论系统 ##
最新版本的hexo已经支持第三方levere插件的一键配置，只需要在主题 _config.yml 文件中找到livere_uid，取消其注释并加上你获取到的livere_uid即可。已经不需要向以前很多博文中介绍的那样，添加修改各种文件和代码了。

---
# 写在最后 #
我的建站历程终于到此为止啦，希望你也建站顺利！在此感谢[AbelChao](http://bbblemon.top/)同学的帮助让我少走了很多弯路！感谢橙子彤倾情赞助的首页美图！

---
# 可供参考的其他建站资料 #
[Hexo+GithubPages&CodingPages搭建自己的个人博客](http://jmyblog.top/Hexo-GithubPages-CodingPages%E6%90%AD%E5%BB%BA%E8%87%AA%E5%B7%B1%E7%9A%84%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/#more)
[GitHub+Hexo 搭建个人网站详细教程](https://www.cnblogs.com/ruruozhenhao/p/8215011.html)
[Hexo搭建博客教程](https://thief.one/2017/03/03/Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B/)
[hexo的next主题个性化教程:打造炫酷网站](https://www.jianshu.com/p/f054333ac9e6)
[next主题配置](http://theme-next.iissnan.com/theme-settings.html)














