#### 搭建步骤
###### 创建Mybatis工程（Maven工程或者普通工程）
- 如果是创建maven工程则直接导入坐标记录
```xml
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
- 如果是普通版的工程，手动导入即可；<br>
jar包下载地址一：[http://search.maven.org/#browse](http://search.maven.org/#browse)<br>
jar包下载地址二：[https://mvnrepository.com/](https://mvnrepository.com/)

###### 创建Mybatis的主配置文件SqlMapConfig.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="db.properties"></properties>
    
    <!--【说明：创建标签顺序不能改变，否则会报下面错误，即必须按照下面的标签顺序】-->
    <!--properties?,settings?,typeAliases?,typeHandlers?,objectFactory?,objectWrapperFactory?,reflectorFactory?,plugins?,environments?,databaseIdProvider?,mappers?-->

    <!--【打印查询语句】-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING" />
    </settings>

    <!--【配置别名】只能配置Bean中的别名，type-全限定类名-->
    <typeAliases>
        <!--第一种方法：使用全限定类名,仅针对单个类，多个的话，则需要编写多个-->
        <!--<typeAlias alias="User" type="com.bean.User" />-->
        <!--第二种方法：直接使用包名，指定的是实体类-->
        <package name="com.bean"/>
    </typeAliases>
   
    <!--【jdbc驱动设置】-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <!--1.POOLED-连接池；2.UNPOOLED-没有连接池概念-->
            <dataSource type="POOLED">
                <!--直接配置-->
                <!--<property name="driver" value="com.mysql.jdbc.Driver" />-->
                <!--<property name="url" value="jdbc:mysql://127.0.0.1:3306/laravelblog" />-->
                <!--<property name="username" value="root" />-->
                <!--<property name="password" value="root" />-->
                 
                <!--使用属性文件配置-->
                <property name="driver" value="${jdbc.driver}" />
                <property name="url" value="${jdbc.url}" />
                <property name="username" value="${jdbc.username}" />
                <property name="password" value="${jdbc.password}" />
            </dataSource>
        </environment>
    </environments>
    <!--【映射器配置】-->
    <mappers>
        <!--基于XML的配置-->
        <!--【xml配置】（类路径的资源引用）  -->
        <mapper resource="com/dao/IUserDao.xml" />
        
        <!--基于接口注解的配置-->
        <!--【映射器接口配置-完全限定类名】使用接口映射器（配置多个时需要编写多个）-->
        <!--<mapper class="com.dao.IUserDao" />-->
      
        <!-- 【映射器接口配置-包名全限定】（将包内的映射器接口实现全部注册为映射器）使用包路径，注册全部接口映射，包中的接口映射器，规则必须统一，否则会抛出异常【指接口路径】 -->
        <!--<package name="com.dao" />-->
        
    </mappers>
</configuration>
```

###### 创建映射配置文件
- 基于xml的配置文件[IUserDao.xml]
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--全限定接口名-->
<mapper namespace="com.dao.IUserDao">
      <!--  
            当查询属性名和数据库列名不一致的情况
            第一种方式：取别名，在数据库的sql语句上解决问题；
            第二种方式：配置列名和实体属性名不一致的问题,使用如下方式，
            但是查询的结果类型的变程resultMap="userMap"
        -->  
    <resultMap id="userMap" type="com.bean.User">-->
        <!--主键的写法-->
        <id property="id" column="id"></id>-->
        <!--非主键的写法-->
        <result property="username" column="username"></result>
     </resultMap>
    
    <!--了解的内容，抽取重复的Sql语句-->
    <sql id="defaultUser">
        select * from user
    </sql>

    <!--配置查询所有-->
    <select id="findAll" resultType="User">
        <!--导入抽取重复的sql语句-->
        <include refid="defaultUser"></include>
        select * from user
    </select>

    <!--查询单条-->
    <select id="findOne" resultType="User">
        select * from user where id=#{id}
    </select>

    <!--模糊查询-->
    <select id="findLike" parameterType="java.lang.String" resultType="User">
        <!--第一种查询方法,条件需要加上%（多用第一种）：主要使用的是PrepatedStatement的参数占位符-->
        select * from user where username like #{aa}
        <!--第二种查询方法,不需要百分号：主要使用的是Statement对象的字符串拼接方式-->
        <!-- select * from user where username like '%${value}%'-->
    </select>

    <!--总用户查询-->
    <select id="findCount" resultType="int">
        select count(*) from user
    </select>


    <!--查询不确定的条件-->
    <!--总用户查询-->
    <select id="findUserByCondition" parameterType="User" resultType="User">
        select * from user
        <where>
            <if test="username != null" >
                and username = #{username}
            </if>
            <if test="id !=null" >
                and id= #{id}
            </if>
        </where>
    </select>

    <!--根据In条件(1,2,3)查询-->
    <select id="findUserIn" parameterType="User" resultType="User">
        select * from user
        <where>
            <if test="ids != null and ids.size>0">
                <!--id-->
                <foreach collection="ids" open="and id in(" close=")" item="uid" separator=",">
                    #{uid}
                </foreach>
            </if>
        </where>
    </select>
    
    <!--【获取插入主键ID的写法】-->
    <!--【方法一】保存用户【配置插入数据的ID号】-->
    <insert id="saveUser" parameterType="User">
        <!--【配置插入数据的ID号】【keyColumn表示数据库id号】【keyProperty表示实例类型ID号】-->
        <selectKey keyProperty="id" keyColumn="id" resultType="int" order="AFTER">
            select last_insert_id();
        </selectKey>
        insert into user(username,address,sex,birthday) values(#{username},#{address},#{sex},#{birthday})
    </insert>

    <!--【方法二】保存用户【配置插入数据的ID号】-->
    <insert id="saveUser" parameterType="User" useGeneratedKeys="true" keyProperty="id">
        insert into user(username,address,sex,birthday) values(#{username},#{address},#{sex},#{birthday})
    </insert>

    <!--更新数据    -->
    <update id="updateUser" parameterType="User">
        update user set username=#{username},address=#{address},sex=#{sex},birthday=#{birthday} where id=#{id}
    </update>

    <!--删除数据-->
    <delete id="deleteUser" parameterType="int">
        delete from user where id=#{id}
    </delete>
        
</mapper>
```

###### 测试Mybatis
1. 读取配置文件：`Reader reader=new Resources.getResourceAsReader(SqlMapConfig.xml)`
2. 创建SqlSessionFactory工厂：`SqlSessionFactory sqlSessionFactory=new SqlSessionFactoryBuilder().build(reader);`
3. 使用工厂生产SqlSession对象：`SqlSession session = sqlSessionFactory.openSession();`
4. 使用SqlSession创建Dao接口：`IUserDao userDao = session.getMapper(IUserDao.class);`
5. 使用代理对象执行方法：`List<User> userlist=userdao.findAll();`

###### 完成

- 参考官网：[https://mybatis.org/mybatis-3/zh/index.html](https://mybatis.org/mybatis-3/zh/index.html)

- 参考案例：[https://gitee.com/-/ide/project/victor_tangqing/mybatis_configuration_analysis/edit/master/-/src/main/resources/com/dao/IUserDao.xml](https://gitee.com/-/ide/project/victor_tangqing/mybatis_configuration_analysis/edit/master/-/src/main/resources/com/dao/IUserDao.xml)