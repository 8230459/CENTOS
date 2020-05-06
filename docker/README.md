
```
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
```
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
```
yum install docker-ce docker-ce-cli containerd.io
systemctl start docker
systemctl restart docker
systemctl stop docker
systemctl status docker
docker run hello-world
<<<<<<< HEAD
```
### 创建镜像
```
cd ~
wget http://.../apache-tomcat-8.5.6.tar.gz
wget http://.../jdk7u79linuxx64.tar.gz
tar -zxvf apache-tomcat-8.5.6.tar.gz
mv apache-tomcat-8.5.6.tar.gz tomcat8
tar -zxvf jdk7u79linuxx64.tar.gz
mv jdk7u79linuxx64.tar.gz jdk1.7
vi Dockerfile
```
```
FROM centos
MAINTAINER andy "8230459@qq.com"
RUN mkdir -p /home/java/jdk1.7
COPY jdk1.7 /home/java/jdk1.7
RUN mkdir -p /home/java/tomcat8
COPY tomcat8 /home/java/tomcat8
ENV JAVA_HOME /home/java/jdk1.7
ENV JRE_HOME /home/java/jdk1.7/jre
ENV PATH $PATH:$JAVA_HOME/bin:$JRE_HOME/bin
EXPOSE 8080
CMD ["/home/java/tomcat8/bin/catalina.sh","run"]
```
> 在192.168.1.6的mysql中新建名为activiti6ui数据库
```
vi activiti-app/WEB-INF/classes/META-INF/activiti-app/activiti-app.properties
```
```
#datasource.driver=org.h2.Driver
#datasource.url=jdbc:h2:mem:activiti;DB_CLOSE_DELAY=-1

datasource.driver=com.mysql.jdbc.Driver
datasource.url=jdbc:mysql://192.168.1.6:3306/activiti6ui?characterEncoding=UTF-8

datasource.username=root
datasource.password=123456

#hibernate.dialect=org.hibernate.dialect.H2Dialect
hibernate.dialect=org.hibernate.dialect.MySQLDialect
#hibernate.dialect=org.hibernate.dialect.Oracle10gDialect
#hibernate.dialect=org.hibernate.dialect.SQLServerDialect
#hibernate.dialect=org.hibernate.dialect.DB2Dialect
#hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```
```
docker build -t activiti6:0.0.1 .
docker run -d -p 8081:8080 --name activiti6 activiti6:0.0.1
docker images
docker ps -a
netstat -tnlp
```

> 访问http://服务器IP:8081/activiti-app

=======
```
>>>>>>> parent of a03a0f6... no msg
