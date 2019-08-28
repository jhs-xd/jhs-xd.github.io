---
title: 常用Linux命令
copyright: true
categories: 
  - Linux
tags: 
  - Linux命令

---
<blockquote class="blockquote-center">All that you do, do with your might; things done by halves are never done right.——R.H. Stoddard
![](/myimages/17.jpg)

<!-- more -->

---


	1. 查看进程			  ps -fu [用户名]
	2. 杀进程	 	 	  kill -9 [进程号]
	3. 查看一个程序是否运行	ps –ef|grep tomcat （查看所有有关tomcat的进程）
	   显示所有用户所有终端的所有程序  ps -aux
	
	4. 清缓存				rm -rf Catalina （提示：一定要在tomcat的work目录下执行此命令）
	
	5. 重启项目			  ./startup.sh
	
	6. 查看日志			  tail -f catalina.out
	7. 查找日志记录		  vi + catalina
	
	8. 当前工作目录的绝对路径	pwd
	9. 创建目录				mkdir newfolder
	10. 删除空目录			rmdir deleteEmptyFolder
	11. 递归删除目录中所有内容	rm -rf deleteFile

	12. 查看文件，包含隐藏文件	ls -al
	13. 复制文件				cp source dest
	14. 递归复制整个文件夹		cp -r sourceFolder targetFolder
	15. 远程拷贝				scp sourecFile romoteUserName@remoteIp:remoteAddr
	16. 移动文件				mv /temp/movefile /targetFolder
	17. 重命令				mv oldNameFile newNameFile
	18. 压缩文件				tar -czf test.tar.gz /test1 /test2
	19. 列出压缩文件列表		tar -tzf test.tar.gz
	20. 解压文件				tar -xvzf test.tar.gz
	
	21. 查看文件头10行		  head -n 10 example.txt
	22. 查看文件尾10行		  tail -n 10 example.txt
	23. 查看日志类型文件		tail -f exmaple.log	（这个命令会自动显示新增内容，屏幕只显示10行内容的（可设置））
	24. 使用管理员身份删除文件	sudo rm a.txt
	25. 修改文件权限			chmod 777 file.java （file.java的权限-rwxrwxrwx，r表示读、w表示写、x表示可执行）
	
	26. 查找文件
		find / -name filename.txt 根据名称查找/目录下的filename.txt文件。
		find . -name "*.xml" 递归查找所有的xml文件
		find . -name "*.xml" |xargs grep "hello world" 递归查找所有文件内容中包含hello world的xml文件
		grep -H 'spring' *.xml 查找所有的包含spring的xml文件
		find ./ -size 0 | xargs rm -f &amp; 删除文件大小为零的文件
		ls -l | grep '.jar' 查找当前目录中的所有jar文件
		grep 'test' d* 显示所有以d开头的文件中包含test的行。
		grep 'test' aa bb cc 显示在aa，bb，cc文件中匹配test的行。
		grep '[a-z]\{5\}' aa 显示所有包含每个字符串至少有5个连续小写字符的字符串的行。

	27. 切换用户				su -username
	28. 查看端口占用情况		netstat -tln | grep 8080 查看端口8080的使用情况
	29. 查看端口属于哪个程序	lsof -i :8080
	30. 网络检测				ping www.just-ping.com
	31. 远程登录				ssh userName@ip
	32. 打印信息				echo $JAVA_HOME 打印java home环境变量的值