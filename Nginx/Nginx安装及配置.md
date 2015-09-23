## Nginx 安装及配置
1. ### 安装Nginx
	`apt-get install nginx-full`

2. ### 测试
> 在浏览器地址栏输入机器IP，如果显示 `Welcome to nginx!` 类似字样，说明 Nginx 安装成功

3. ### 配置 Nginx
	1. ##### 编辑 Nginx 的配置文件
	`vim /etc/nginx/nginx.conf`
	2. ##### 配置文件内容如下  
	
	```
	#运行用户 
	user  root root;
	#启动进程
	worker_processes  10;
	#全局错误日志及PID文件 
	error_log /var/log/nginx/error.log;
	pid /run/nginx.pid;
	#worker进程的最大打开文件数限制
	worker_rlimit_nofile 51200;
	#工作模式及连接数上限
	events {
	    use epoll; 
	    worker_connections  51200;
	}
	#设定http服务器，利用它的反向代理功能提供负载均衡支持 
	http {
	    #设定mime类型 
	    include       mime.types;
	    default_type  application/octet-stream;
    	sendfile        on;
    	#tcp_nopush     on;
    	#keepalive_timeout  0;
    	#客户端和nginx之间空闲链接超时时间
    	keepalive_timeout  120;
		tcp_nodelay on;
	    fastcgi_connect_timeout 300;
	    fastcgi_send_timeout 300;
	    fastcgi_read_timeout    300;
	    fastcgi_buffer_size 64k;
	    fastcgi_buffers 4 64K;
	    fastcgi_busy_buffers_size 128k;
	    fastcgi_temp_file_write_size 128k; 
	    gzip  on;
	    gzip_min_length  1K;
	    gzip_buffers    4 16k;
	    gzip_http_version 1.1;
	    gzip_comp_level 2;
	    gzip_types      text/plain application/x-javascript text/css application/xml; 
	    gzip_vary on;
	    #客户端body中最大数据量
	    client_max_body_size    300m;
	    client_body_buffer_size 512k;
	    large_client_header_buffers 4 32k;
	    # 链接到主机超时时间
	    proxy_connect_timeout  600;
	    # 向主机发送数据超时时间
	    proxy_send_timeout      600;
	    # 从主机读取数据超时时间
	    proxy_read_timeout      600;
	    proxy_buffer_size      16k;
	    proxy_buffers          4 32k;
	    proxy_busy_buffers_size 64k;
	    proxy_headers_hash_max_size 51200;
	    proxy_headers_hash_bucket_size 6400;
	    proxy_temp_file_write_size 64k;
	    #设定负载均衡的服务器列表
	    upstream ssapServices {
	    	server 10.200.26.200:8080 weight=1 max_fails=3 fail_timeout=30s;
	       	server 10.200.26.201:8080 weight=1 max_fails=3 fail_timeout=30s;
	       	server 10.200.26.202:8080 weight=1 max_fails=3 fail_timeout=30s;
	       	ip_hash;		
    	}
    	log_format  main  '<<<reqtime: $time_local reqip: $remote_addr '  
                        'ups_resp_ip: $upstream_addr '  
                        'ups_resp_time: $upstream_response_time '  
                        'request_time: $request_time>>>';  
	    #设定虚拟主机 
	    server {
	        listen       80;
	        server_name  kylin;
			index index.html index.htm index.jsp default.jsp index.do defautl.do;
			root /home/zhaolong/zlong/apache-tomcat-6.0.41/webapps;
	        #对 "/" 启用负载均衡
	        location /{
	            proxy_next_upstream http_502 http_504 error timeout invalid_header;
				proxy_redirect off;
	            proxy_pass      http://ssapServices;
	            proxy_set_header        Host $host;
	            proxy_set_header       X-Real-IP $remote_addr;
	            #proxy_set_header        X-Forwarded-For $remote_addr;
	            proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
				proxy_set_header     Cookie   $http_cookie;
				chunked_transfer_encoding   off;            
        	}
	        #生产期 access_log off
	        access_log  /var/log/nginx/access.log;
	        #access_log off
    	}
	}
	```
