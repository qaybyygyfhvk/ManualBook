## Redis 集群配置

1. ### 集群配置目标：
	* 机器1：
		> IP:192.168.1.100 
		> Redis端口1：7000 
		> Redis端口2：7001 
	
	* 机器2：
		> IP:192.168.1.101
		> Redis端口1：7000
		> Redis端口2：7001
	
	* 机器3：
		> IP:192.168.1.102
		> Redis端口1：7000
		> Redis端口2：7001

2. ### 集群配置
	1. #### 安装依赖 
		1. 安装Ruby 
		`apt-get install ruby-full`

		2. 安装gem
		`gem install redis`

		> 如果上述过程没有源，不能进行安装，可以操作如下步骤
		```
		gem sources --remove https://rubygems.org/
		gem sources -a https://ruby.taobao.org/
		gem sources -l
		gem install redis
		```  
	2. #### 创建配置文件  
		1. 进入 /usr/local  
		`cd /usr/local`  
		2. 创建集群目录 redis-cluster  
		`mkdir redis-cluster`  
		3. 进入 redis-cluster  
		`cd redis-cluster`   
		4. 编写端口7000实例的配置文件  
		`vi 7000.conf`  
		> 添加如下内容
		```
		port 7000
		daemonize yes
		pidfile /var/run/redis_7000.pid
		timeout 0
		loglevel notice
		logfile /usr/local/redis-cluster/redis_7000.log
		save 900 1
		save 300 10
		save 60 10000
		cluster-enabled yes
		cluster-config-file /usr/local/redis-cluster/node-7000.conf
		cluster-node-timeout 5000
		appendonly yes
		dir  /usr/local/redis-cluster/
		dbfilename dump_7000.rdb
		appendfilename appendonly_7000.aof
		```

		5. 编写端口7001实例的配置文件
		`vi 7001.conf`
		> 添加如下内容
		```
		port 7001
		daemonize yes
		pidfile /var/run/redis_7001.pid
		timeout 0
		loglevel notice
		logfile /usr/local/redis-cluster/redis_7001.log
		save 900 1
		save 300 10
		save 60 10000
		cluster-enabled yes
		cluster-config-file /usr/local/redis-cluster/node-7001.conf
		cluster-node-timeout 5000
		appendonly yes
		dir  /usr/local/redis-cluster/
		dbfilename dump_7001.rdb
		appendfilename appendonly_7001.aof
		```  

	3. #### 创建启动脚本
		`vi redis-start` 
		> 添加如下内容
		```
		redis-server /usr/local/redis-cluster/7000.conf
		redis-server /usr/local/redis-cluster/7001.conf
		```

	4. #### 赋权
		`chmod a+x redis-start`  
	5. #### 复制集群配置文件
		```
		scp -r redis-cluster root@192.168.1.101:/usr/local/
		scp -r redis-cluster root@192.168.1.102:/usr/local/
		```
	6. #### 启动所有实例
		`./redis-start`

3. ### 创建 Redis 集群
	1. 执行如下命令
	`redis-trib.rb create --replicas 1 192.168.1.100:7000 192.168.1.100:70001 192.168.1.101:7000 192.168.1.101:7001 192.168.1.102:7000 192.168.1.102:7001`