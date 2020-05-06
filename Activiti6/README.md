
### 配置
- mysql版本必须是5.x
- jdk和jre的环境变量分别是：
```
jdk1.8.0_202\bin
jdk1.8.0_202\jre
```
- 先将war文件放在apache-tomcat-8.5.45\webapps下执行startup.bat后访问http://localhost:8080/activiti-app/看是否正常

- 执行shutdown.bat和shutdown.sh后，将war文件删除

- 创建数据库，名字叫activiti6ui

- 修改配置文件 apache-tomcat-8.5.45\webapps\activiti-app\WEB-INF\classes\META-INF\activiti-app\activiti-app.properties
```
#datasource.driver=org.h2.Driver
#datasource.url=jdbc:h2:mem:activiti;DB_CLOSE_DELAY=-1

datasource.driver=com.mysql.jdbc.Driver
datasource.url=jdbc:mysql://localhost:3306/activiti6ui?nullCatalogMeansCurrent=true&characterEncoding=UTF-8

datasource.username=root
datasource.password=123456

#hibernate.dialect=org.hibernate.dialect.H2Dialect
hibernate.dialect=org.hibernate.dialect.MySQLDialect
#hibernate.dialect=org.hibernate.dialect.Oracle10gDialect
#hibernate.dialect=org.hibernate.dialect.SQLServerDialect
#hibernate.dialect=org.hibernate.dialect.DB2Dialect
#hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

- 执行startup.bat后访问http://localhost:8080/activiti-app/看是否正常

- activiti-app登录初始账号是：admin/test