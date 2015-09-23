## Tomcat 配置
1. ### 获取 tomcat 
`wget http://219.239.26.11/files/3237000006CC102A/mirror.bit.edu.cn/apache/tomcat/tomcat-7/v7.0.64/bin/apache-tomcat-7.0.64.tar.gz`
2. ### 解压至 /usr/local/ 目录下
`tar zxvf apache-tomcat-7.0.64.tar.gz -C /usr/local`
3. ### 进入解压目录
`cd /usr/local/apache-tomcat-7.0.64`
4. ### 配置Http连接参数
`vim conf/server.xml`
> 配置内容如下
```xml
<Connector port="8080" protocol="HTTP/1.1" 
    redirectPort="8443" URIEncoding="UTF-8" 
	maxHttpHeaderSize="8192" maxThreads="1000"  
	minSpareThreads="100"  maxSpareThreads="1000"  
	minProcessors="100"  maxProcessors="1000"  
	enableLookups="false"  compression="on"  
	compressionMinSize="2048"  
	compressableMimeType="text/html,text/xml,text/javascript,text/css,text/plain,application/xml,application/json"
	connectionTimeout="20000"  acceptCount="1000"  disableUploadTimeout="true" />
```
5. ### 配置JVM参数
`vim bin/catalina.sh`
> 插入如下内容
```
JAVA_OPTS=%JAVA_OPTS% -Xms128m -Xmx2048m -XX:MaxNewSize=256m -XX:MaxPermSize=1024m
```