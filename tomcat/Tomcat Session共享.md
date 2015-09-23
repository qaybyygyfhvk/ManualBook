## Tomcat Session共享

1. ### 安装Memcached  
	1. Windown下安装  [memcached-win64-1.4.4-14.zip](http://s3.amazonaws.com/downloads.northscale.com/memcached-win64-1.4.4-14.zip)  
	> 1) 解压memcached-win64-1.4.4-14.zip至任意目录  
	> 2) 以管理员身份运行cmd，cd上面的解压目录  
	> 3) 输入命令memcached -d install进行安装  
	> 4) 输入命令memcached -d start来启动memcached  

	2. Linux下安装  
	> 1) apt-get install memcached  
	> 2) memcached -d -p 11211  
 
2. ### Tomcat配置
	1. 下载所需的jar包  
	[memcached-session-manager-1.8.3.jar](http://central.maven.org/maven2/de/javakaffee/msm/memcached-session-manager/1.8.3/memcached-session-manager-1.8.3.jar)  
	[memcached-session-manager-tc7-1.8.3.jar](http://central.maven.org/maven2/de/javakaffee/msm/memcached-session-manager-tc7/1.8.3/memcached-session-manager-tc7-1.8.3.jar)  
	[javolution-5.5.1.jar](http://central.maven.org/maven2/javolution/javolution/5.5.1/javolution-5.5.1.jar)  
	[spymemcached-2.12.0.jar](http://central.maven.org/maven2/net/spy/spymemcached/2.12.0/spymemcached-2.12.0.jar)  
	[msm-javolution-serializer-1.8.3.jar](http://central.maven.org/maven2/de/javakaffee/msm/msm-javolution-serializer/1.8.3/msm-javolution-serializer-1.8.3.jar)  
	[msm-kryo-serializer-1.8.3.jar](http://central.maven.org/maven2/de/javakaffee/msm/msm-kryo-serializer/1.8.3/msm-kryo-serializer-1.8.3.jar)  
	[msm-javolution-serializer-1.8.3.jar](http://central.maven.org/maven2/de/javakaffee/msm/msm-javolution-serializer/1.8.3/msm-javolution-serializer-1.8.3.jar)  
	 
	2. 将上面的所有jar包放到tomcat/lib目录下  
	3. 配置context.xml文件  
	```xml
	<Context>
		<Manager
			className="de.javakaffee.web.msm.MemcacheBackupSessionManager"
			memcachedNodes="n1:10.200.32.64:11211"
			sessionBackupAsync="false"
			sessionBackupTimeout="1800000"
      		copyCollectionsForSerialization="false"
      		transcoderFactoryClass="de.javakaffee.web.msm.serializer.javolution.JavolutionTranscoderFactory"/>
	</Context>
	```

	4. 编写测试jsp  
	```JSP
	SessionID:<%=session.getId()%>  
	<BR>  
	SessionIP:<%=request.getServerName()%>  
	<BR>  
	SessionPort:<%=request.getServerPort()%>  
	<BR>
	```
	5. 分别启动tomcat进行测试，如果访问上述jsp文件显示sessionID一致，则说明session共享成功
	