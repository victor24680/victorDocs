SSM整合<br />
- 整合的思想
1. 使用Spring框架整合SpringMVC框架 <br />
2. 使用Spring框架整合Mybatis框架 <br />
## **Jar依赖包** <br />
```
<!--【数据相关】-->
<!--【MySql连接】-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.6</version>
</dependency>
<!--【Mybatis-Srping】-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.4</version>
</dependency>
<!--【Mybatis】-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.5</version>
</dependency>
<!--【C3po数据库连接】-->
<dependency>
    <groupId>com.mchange</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.5.2</version>
</dependency>
```

整合的步骤
1. 搭建整合的环境
2. 搭建Spring配置环境
3. 使用Spring整合SpringMVC框架
4. 使用Spring整合Mybatis框架
## Spring整合SpringMVC
**整合流程**<br />
- 搭建SpringMVC环境(前面已介绍，此处忽略)
- 编写Spring业务类
```
package com.service;

import com.bean.User;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;
//添加注解，使之注册为服务类
@Service("userService")
public class UserService {
    //正常情况下，需要使用接口来约束
    //查询数据
    public List<User> findAll() {
        List<User> list = new ArrayList<User>();
        //模拟数据库查询
        User user = new User();
        user.setId(1);
        user.setBirthday(new Date());
        user.setUsername("java1");
        user.setSex("男");
        user.setAddress("中国深圳");
        list.add(user);

        return list;
    }
}
```
- 配置业务类的扫描配置`applicationContext.xml`
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:jdbc="http://www.springframework.org/schema/jdbc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
     http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd
     http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
     http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-3.0.xsd
     http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
     http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
     http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
     <!--上面的约束是比较完整的约束，开发中可以使用此约束-->
    <context:annotation-config />
    <!-- 开启注解的扫描，希望处理 service和dao、bean等-->
    <context:component-scan base-package="com.service" />
    <context:component-scan base-package="com.dao" />
    <context:component-scan base-package="com.bean" />

    <!--如果希望不扫描某个注解，使用下面的配置方法即可-->
    <!--
    <context:component-scan base-package="com.*">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    -->
</beans>
```
- `web.xml`文件中配置监听器，使之服务器监听`applicationContext.xml`文件<br />
Spring整合SpringMVC<br />
启动tomcat服务器的时候，需要加载spring配置文件，创建`ServeltContext`域对象，服务器关闭，域对象则才销毁.
```
#web.xml
<!-- 监听applicationContext.xml   -->
<!-- 配置Spring监听器：默认只加载WEB-INF目录的applicationContext.xml配置文件 -->
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!-- 设置配置文件的路径 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```
- 控制测试
完成上面两个步骤之后，可以依赖注入了
```
/**
 * 测试整合
 */
@Controller
@RequestMapping("Index")
public class IndexController {
    @Autowired //自动注入
    private UserService userService;

    @RequestMapping(value = "/testSSM")
    public String testSSM(){//自动注入之后，即可调用业务层的方法
       //User类，自行编写，此处忽略；
       List<User> users=userService.findAll();
       for (User user:users) {
            System.out.println("------------------------------------------");
            System.out.println(user.getId());
            System.out.println(user.getUsername());
            System.out.println(user.getSex());
            System.out.println(user.getBirthday());
            System.out.println(user.getAddress());
            System.out.println("------------------------------------------");
       }
       return "home/Index/success";
    }
}
```
## Spring整合Mybatis
- 创建数据表
```
CREATE TABLE `users` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) DEFAULT NULL,
  `birthday` date DEFAULT NULL,
  `sex` varchar(10) DEFAULT NULL,
  `address` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=0 DEFAULT CHARSET=utf8;
```
- 创建Dao数据接口
```
package com.dao;

import com.bean.User;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.springframework.stereotype.Repository;
import java.util.List;
@Repository("userDao")
public interface UserDao {
    //查询服务
    @Select("select * from users")
    public List<User> findAll();

    //数据新增
    @Insert("insert into users(username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})")
    public void insertData(User user);
}
```
- 创建数据业务类

```
package com.service;

import com.bean.User;
import com.dao.UserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.transaction.interceptor.TransactionAspectSupport;
import java.util.List;

@Service("userService")
public class UserService {
    @Autowired //自动注入数据查询接口
    UserDao userDao;

    //查询数据
    public List<User> findAll() {
        List<User> list=userDao.findAll();
        return list;
    }

    //插入数据管理
    //使用注解的方式配置时才使用此配置
    @Transactional(propagation= Propagation.REQUIRED,rollbackForClassName="Exception")
    public void insertData(User user){
        userDao.insertData(user);
        //【手动回滚事物】
	TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
    }
}
```
- `applicationContext.xml`中配置数据驱动
1. 配置数据源
2. 配置事物管理
```
<!----------------【数据源配置】---------------------------------------------->
<!--配置数据库-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName">
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <property name="url">
        <value>jdbc:mysql://127.0.0.1:3306/laravelblog?characterEncoding=UTF-8</value>
    </property>
    <property name="username">
        <value>root</value>
    </property>
    <property name="password">
        <value>123123</value>
    </property>
</bean>

<!--创建SqlSessionFactory工厂 -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="typeAliasesPackage" value="com.bean" />
    <property name="dataSource" ref="dataSource"/>
    <!-- 使用XML方式配置：CRUD，需要与下面的【属性1 或者 属性2】配和使用-->
    <!--<property name="mapperLocations" value="classpath:com/config/*.xml" />-->
</bean>

<!--配置dao包接口 【即扫描包的方式配置（也可以单独配置）】-->
<bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <!--如果使用XML配置的方式查询，则需要使用配置下面-属性：SqlSessionFactory，或者属性：sqlSessionFactoryBeanName-->
    <!--属性1-->
    <!--<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />-->
    <!--属性2-->
    <!--<property name="sqlSessionFactory" ref="sqlSessionFactory" />-->

    <!--此处测试演示：用的是注解配置的方式：CRUD-->
    <property name="basePackage" value="com.dao" />
</bean>

<!----------------【事物管理配置】---------------------------------------------->
<!--【事物管理】：注解的方式配置-（需要在业务服务类作相应的配置）-->
<tx:annotation-driven transaction-manager="transactionManager" />
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource" />
</bean>
<!--【事物管理】：XML方式配置-->
<!--
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource" />
</bean>

<tx:advice id="txadvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="insertData*" propagation="REQUIRED" rollback-for="Exception" />
    </tx:attributes>
</tx:advice>

<aop:config>
    <aop:pointcut id="serviceMethod" expression="execution(* com.service.UserService.*(..))"/>
    <aop:advisor pointcut-ref="serviceMethod" advice-ref="txadvice"/>
</aop:config>
-->
```














