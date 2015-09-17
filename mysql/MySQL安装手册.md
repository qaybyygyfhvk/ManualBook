## MySQL安装手册
1. 获取安装文件：[mysql-5.6.26-linux-glibc2.5-x86_64.tar.gz](http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.26-linux-glibc2.5-x86_64.tar.gz)

2. 复制tar文件到 /usr/local/ 目录下  
	`cp mysql-5.6.26-linux-glibc2.5-x86_64.tar.gz /usr/local`

3. 转转至 /usr/local 目录  
    `cd /usr/local`

4. 解压tar包  
	`tar zxvf mysql-5.6.26-linux-glibc2.5-x86_64.tar.gz`

5. 添加名为 mysql 指向解压的文件夹的软连接  
	`ln -s mysql-5.6.26-linux-glibc2.5-x86_64 mysql`

6. 安装 mysql 所使用的依赖  
	`apt-get install libaio1`

7. 添加 mysql 用户组  
	`groupadd mysql`

8. 添加 msyql 用户到 mysql 用户组  
	`useradd -r -g mysql mysql`

9. 进入 mysql 目录  
	`cd mysql`

10. 设置 mysql 目录的拥有者和所属的用户组  
	```
    chown -R mysql .
    chgrp -R mysql .
    ```

11. 执行 mysql 安装脚本  
	`scripts/mysql_install_db --user=mysql`

12. 再次设置 mysql 目录的拥有者  
	`chown -R root .`

13. 设置 data 目录的拥有者  
	`chown -R mysql data`

14. 复制 mysql 配置文件  
	`cp support-files/my-default.cnf /etc/my.cnf`

15. 启动 mysql  
	`bin/mysqld_safe --user=mysql &`

16. 设置root密码  
	`bin/mysqladmin -u root password 'root' `

17. 复制mysql.server 脚本  
	`cp support-files/mysql.server /etc/init.d/mysql.server`

18. 查看mysql的状态，如果显示`* MySQL running ` 说明启动成功  
	`service mysql.server status`

19. 把 /usr/local/mysql/bin/mysql 命令加到用户命令中，这样就不用每次都加 mysql命令的路径  
	`ln -s /usr/local/mysql/bin/mysql /usr/local/bin/mysql`

20. 登录 mysql 配置下允许root用户远程登录  
	`mysql -u root -p`  
    `mysql > GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY 'root'  WITH GRANT OPTION;`

21. 设置 mysql 开机启动  
	`update-rc.d -f mysql.server defaults`