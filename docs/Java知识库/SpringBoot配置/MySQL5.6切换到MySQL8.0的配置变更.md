<p style="text-indent:20px;font-size:20px;font-weight:bold;">
    对于一些MySQL版本的升级时，需要切换一些配置，如果不切换配置可能会报数据库密码错误
</p>
# 驱动配置 

- `application.properties`中的驱动配置
```mysql
#spring.datasource.url=jdbc:mysql://127.0.0.1:3306/db_msg?characterEncoding=UTF-8 #【旧】
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/db_msg?rootcharacterEncoding=UTF-8&useSSL=false&serverTimezone=UTC #【新】

#spring.datasource.driver-class-name=com.mysql.jdbc.Driver #【旧】
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver #【新】
```
# jar包配置
- `pom.xml` 中的jar依赖
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.12</version>
    <!--从5.6.12直接变换下即可-->
</dependency>
```

# mybatis-spring-boot版本的依赖
- 暂未发现需要变更