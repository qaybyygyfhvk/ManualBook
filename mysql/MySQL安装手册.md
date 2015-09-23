## MySQL安装手册
1. ### 获取安装文件：
	`wget http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.26-linux-glibc2.5-x86_64.tar.gz`

2. ### 解压tar包  
	`tar zxvf mysql-5.6.26-linux-glibc2.5-x86_64.tar.gz -C /usr/local`

3. ### 进入解压目录  
    `cd /usr/local`

4. ### 添加软连接  
	`ln -s mysql-5.6.26-linux-glibc2.5-x86_64 mysql`

5. ### 安装 mysql 所使用的依赖  
	`apt-get install libaio1`

6. ### 添加 mysql 用户组  
	`groupadd mysql`

7. ### 添加 msyql 用户到 mysql 用户组  
	`useradd -r -g mysql mysql`

8. ### 进入 mysql 目录  
	`cd mysql`

9. ### 设置 mysql 目录的拥有者和所属的用户组  
	```
    chown -R mysql .
    chgrp -R mysql .
    ```

10. ### 执行 mysql 安装脚本  
	`scripts/mysql_install_db --user=mysql`

11. ### 再次设置 mysql 目录的拥有者  
	`chown -R root .`

12. ### 设置 data 目录的拥有者  
	`chown -R mysql data`

13. ### 复制 mysql 配置文件  
	`cp support-files/my-default.cnf /etc/my.cnf`

14. ### 启动 mysql  
	`bin/mysqld_safe --user=mysql &`

15. ### 设置root密码  
	`bin/mysqladmin -u root password 'root' `

16. ### 复制mysql.server 脚本  
	`cp support-files/mysql.server /etc/init.d/mysql.server`

17. ### 查看mysql的状态， 
	`service mysql.server status`
	> 如果显示`* MySQL running ` 说明启动成功 
18. ### 添加 mysql 软链接  
	`ln -s /usr/local/mysql/bin/mysql /usr/local/bin/mysql`

19. ### 登录 mysql 配置下允许root用户远程登录  
	`mysql -u root -p`  
    `mysql > GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY 'root'  WITH GRANT OPTION;`

20. ### 设置 mysql 开机启动  
	`update-rc.d -f mysql.server defaults`