## Redis 安装手册

1. 获取 Redis 安装包  
	`wget http://download.redis.io/releases/redis-3.0.4.tar.gz`

2. 复制 Redis 安装包至目录 /usr/local/ 下  
	`cp redis-3.0.4.tar.gz /usr/local/`

3. 解压 Redis 安装包  
	`tar zxvf redis-3.0.4.tar.gz`

4. 进入 Redis 目录  
	`cd redis-3.0.4`

5. 编译  
	`make`  

	> 如果报如下错误, 则安装gcc, `apt-get install gcc`
	```
	make[3]: gcc: Command not found
	make[3]: *** [net.o] Error 127
	make[3]: Leaving directory `/usr/local/redis-3.0.4/deps/hiredis'
	make[2]: *** [hiredis] Error 2
	make[2]: Leaving directory `/usr/local/redis-3.0.4/deps'
	make[1]: [persist-settings] Error 2 (ignored)
    	CC adlist.o
	/bin/sh: 1: cc: not found
	make[1]: *** [adlist.o] Error 127
	make[1]: Leaving directory `/usr/local/redis-3.0.4/src'
	make: *** [all] Error 2
	```   

	> 如果报如下错误, 执行 `make MALLOC=libc`
	```
	make[1]: Entering directory `/usr/local/redis-3.0.4/src'
	    CC adlist.o
	In file included from adlist.c:34:0:
	zmalloc.h:50:31: fatal error: jemalloc/jemalloc.h: No such file or directory
	 #include <jemalloc/jemalloc.h>
	                               ^
	compilation terminated.
	make[1]: *** [adlist.o] Error 1
	make[1]: Leaving directory `/usr/local/redis-3.0.4/src'
	make: *** [all] Error 2
	```  

6. 配置环境变更  
	`vim /etc/profile`  
	> 添加如下内容 
	```
	export REDIS_HOME=/usr/local/redis-3.0.4
	export PATH=$REDIS_HOME/src:$PATH
	```
7. 更新环境变量  
	`source /etc/profile`  
  
7. 启动 Redis  
	`redis-server`  
	> 启动日志如果出现如下警告
	```
	7603:M 17 Sep 17:44:52.008 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
	```
	> 则执行如下命令  
	`echo 1024 > /proc/sys/net/core/somaxconn`  

	> 启动日志如果出现如下警告  
	```
	7603:M 17 Sep 17:44:52.008 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
	```
	> 则执行如下命令
	`sysctl vm.overcommit_memory=1`
	
	> 启动日志如果出现如下警告  
	```
	7603:M 17 Sep 17:44:52.009 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
	```  
	> 则执行如下命令
	`echo never > /sys/kernel/mm/transparent_hugepage/enabled`

8. 利用 redis-cli 就可以使用redis了，马上试试吧
	`redis-cli`