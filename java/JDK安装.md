## JDK 安装
1. ### 获取安装包
`wget http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz?AuthParam=1442972907_5b4867c470b6e3f72b89c649a8724880`
2. ### 创建目录 /usr/local/jdk
`mkdir -p /usr/local/jdk`
3. ### 解压安装包至 /usr/local/jdk
`tar zxvf jdk-7u79-linux-x64.tar.gz -C /usr/local/jdk` 
4. ### 配置环境变量 `vim /etc/profile`
```
export JAVA_HOME=/usr/local/jdk/jdk1.7.0_79
export JRE_HOME=/usr/local/jdk/jdk1.7.0_79/jre
export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH
export CLASSPATH=$CLASSPATH:.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
```
5. ### 修改默认的jdk
```
sudo update-alternatives --install /usr/bin/java java /usr/local/jdk/jdk1.7.0_79/bin/java 300
sudo update-alternatives --install /usr/bin/javac javac /usr/local/jdk/jdk1.7.0_79/bin/javac 300
sudo update-alternatives --config java
sudo update-alternatives --config javac
```
6. ### 测试jdk是否安装成功
`java -version` 