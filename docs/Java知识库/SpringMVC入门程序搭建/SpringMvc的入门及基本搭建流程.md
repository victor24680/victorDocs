##### 启动服务器加载配置文件
1. 加载web.xml配置文件，然后DispatcherServlet对象被创建
2. springmvc配置文件加载
3. 控制器解析
4. maven工程的话，自行导入响应的jar包即可，普通Web工程，自行下载并导入相应的jar包即可

##### 配置文件
```xml

#web.xml文件配置：WEB-INF/web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" 
    id="WebApp_ID" version="4.0">
  <display-name>Archetype Created Web Application</display-name>
  <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>default.html</welcome-file>
        <welcome-file>default.htm</welcome-file>
        <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>

  <!-- 配置springMVC容器 -->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
      org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <!--位于类资源目录中resources目录，即类路径加载方法-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>

#springmvc.xml配置文件解析方法：resouces/springmvc.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-3.0.xsd">
    <!--扫描控制器包文件-->
    <context:component-scan base-package="com.controller"/>
    <!--注册视图解析器类-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--配置视图目录-->
        <property name="prefix">
            <value>/WEB-INF/jsp/</value>
        </property>
        <!--配置视图文件类型-->
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>

</beans>
```
##### 控制器文件

```java
package com.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller //注册问控制器
@RequestMapping("Index") //注册控制器的类访问名
public class IndexController {
    @RequestMapping(value = "index",method = RequestMethod.GET) //注册控制的方法访问名
    public String index() {
        System.out.println("入门成功");
        //可以自行分目录，比如前台目录的控制器目录下：home/Index/success.jsp
        return "success";
    }
}

```
##### 执行顺序
![输入图片说明](https://images.gitee.com/uploads/images/2020/0413/140035_f17968b3_407852.jpeg "配置图片.jpg")


