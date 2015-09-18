## MySQL主从配置

1. 主机操作
	> 1) 编辑 mysql 的配置文件  
	`vi /etc/my.cnf`  
	>> 在 [mysqld] 中添加如下内容
	```  
	[mysqld]   
	## 至少要有server-id和log-bin两项  
	server-id=1  
	log-bin=mysql-bin  
	## 需要同步的数据库名，如果有多个数据库，可重复此参数，每个数据库一行  
	binlog-do-db=mihsfbase  
	binlog-do-db=mihsfoperation  
	## 不需要同步的数据库名，如果有多个，可重复此参数，一个数据库一行  
	binlog-ignore-db=mysql
	```
	>  
	> 2) 重启mysql数据库，并进入mysql控制台   
	```
	service mysql.server restart  
	mysql -u root -p
	```
	>  
	> 3) 创建一个有复制权限的用户 
	```
	mysql> create user repl_user@从机IP;  
	mysql> grant relpication slave on *.* to repl_user@从机IP identified by '123456';  
	```
	> 
	> 4) 显示Master状态,并记录下面的状态值，在从机配置时需要用到
	```	
	mysql>show master status;
	``` 
	>> 结果显示如下
	```	 
	+------------------+----------+--------------------------+------------------+-------------------+  
	| File             | Position | Binlog_Do_DB             | Binlog_Ignore_DB | Executed_Gtid_Set |  
	+------------------+----------+--------------------------+------------------+-------------------+  
	| mysql-bin.000001 |      120 | mihsfbase,mihsfoperation | mysql            |                   |  
	+------------------+----------+--------------------------+------------------+-------------------+
	```

2. 从机操作
	> 1) 编辑 mysql 配置文件
	`vi /etc/my.cnf`
	>> 在 [mysqld] 中添加如下内容
	```
	[mysqld]  
	server-id=2  
	log-bin=mysql-bin  
	relay-log-index=slave-relay-bin.index  
	relay-log=slave-relay-bin  
	## 需要同步的数据库名，如果有多个数据库，可重复此参数，每个数据库一行  
	replicate-do-db=mihsfbase  
	replicate-do-db=mihsfoperation  
	replicate-ignore-db=mysql
	```
	>
	> 2) 重启mysql数据库，并进入mysql控制台  
	```
	service mysql.server restart  
	mysql -u root -p
	```
	>
	> 3) 设置Master配置  
	>> 停止同步进程  
	`mysql>stop slave;`  
	>> 设置Master  
	```
	mysql> change master to master_host='主机IP',  
	->master_user='repl_user',  
	->master_password='123456',
	->master_log_file='mysql-bin.000001',  
	->master_log_pos=120;
	```  
	>> 开户同步进程  
	`mysql>start slave;`  
	>> 查看slave同步信息，出现的内容中，如果Slave_IO_Running: Yes和Slave_SQL_Running: Yes即为主从配置成功  
	`mysql>SHOW SLAVE STATUS\G`
	>> 结果显示
	```
	*************************** 1. row ***************************
	               Slave_IO_State: Waiting for master to send event
	                  Master_Host: 10.200.32.117
	                  Master_User: repl_user
	                  Master_Port: 3306
	                Connect_Retry: 60
	              Master_Log_File: mysql-bin.000051
	          Read_Master_Log_Pos: 1068
	               Relay_Log_File: slave-relay-bin.000049
	                Relay_Log_Pos: 1231
	        Relay_Master_Log_File: mysql-bin.000051
	             Slave_IO_Running: Yes
	            Slave_SQL_Running: Yes
	              Replicate_Do_DB: mihsfbase,mihsfoperation
	          Replicate_Ignore_DB: mysql
	           Replicate_Do_Table:
	       Replicate_Ignore_Table:
	      Replicate_Wild_Do_Table:
	  Replicate_Wild_Ignore_Table:
	                   Last_Errno: 0
	                   Last_Error:
	                 Skip_Counter: 0
	          Exec_Master_Log_Pos: 1068
	              Relay_Log_Space: 1567
	              Until_Condition: None
	               Until_Log_File:
	                Until_Log_Pos: 0
	           Master_SSL_Allowed: No
	           Master_SSL_CA_File:
	           Master_SSL_CA_Path:
	              Master_SSL_Cert:
	            Master_SSL_Cipher:
	               Master_SSL_Key:
	        Seconds_Behind_Master: 0
	Master_SSL_Verify_Server_Cert: No
	                Last_IO_Errno: 0
	                Last_IO_Error:
	               Last_SQL_Errno: 0
	               Last_SQL_Error:
	  Replicate_Ignore_Server_Ids:
	             Master_Server_Id: 113
	                  Master_UUID: 3dad252c-4bd3-11e5-bbf7-000c29be5e2b
	             Master_Info_File: /usr/local/mysql-advanced-5.6.25-linux-glibc2.5-x86_64/data/master.info
	                    SQL_Delay: 0
	          SQL_Remaining_Delay: NULL
	      Slave_SQL_Running_State: Slave has read all relay log; waiting for the slave I/O thread to update it
	           Master_Retry_Count: 86400
	                  Master_Bind:
	      Last_IO_Error_Timestamp:
	     Last_SQL_Error_Timestamp:
	               Master_SSL_Crl:
	           Master_SSL_Crlpath:
	           Retrieved_Gtid_Set:
	            Executed_Gtid_Set:
	                Auto_Position: 0
	1 row in set (0.01 sec)
	```
