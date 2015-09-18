## Nginx 安装及配置
1. 安装Nginx
	1. 在线安装
	`apt-get install nginx-full`
	2. tar包安装
		1. 下载tar包
		`wget http://nginx.org/download/nginx-1.9.4.tar.gz`
		2. 拷贝至目录 /usr/local
		`cp nginx-1.9.4.tar.gz /usr/local`
		3. 进入目录 /usr/local
		`cd /usr/local`
		4. 解压
		`tar zxvf nginx-1.9.4.tar.gz`
		5. 进入解压目录 nginx-1.9.4.tar.gz
		`cd nginx-1.9.4`
		6. 执行 `./configure`
		> 如果提示如下错误，则需要[安装 PCRE](#pcre) 
		`./configure: error: the HTTP rewrite module requires the PCRE library.`

2. 在浏览器地址栏输入机器IP，如果显示 `Welcome to nginx!` 类似字样，说明 Nginx 安装成功

3. 配置 Nginx
	1. 编辑 Nginx 的配置文件
	`vim /etc/nginx/nginx.conf`
	2. 配置文件内容如下
	```
	#运行用户 
	user  root root;
	#启动进程
	worker_processes  10;
	#全局错误日志及PID文件 
	error_log /var/log/nginx/error.log;
	```

###　附录

* <span id="prce">PCRE 安装<span>
	1. 获取tar包
	`wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre2-10.10.tar.gz`
	2. 拷贝至目录 /usr/local
	`cp pcre2-10.10.tar.gz /usr/local`
	3. 进入目录 /usr/local
	`cd /usr/local`
	4. 解压
	`tar zxvf pcre2-10.10.tar.gz`
	5. 进入解压目录 pcre2-10.10
	`cd pcre2-10.10`
	6. 执行