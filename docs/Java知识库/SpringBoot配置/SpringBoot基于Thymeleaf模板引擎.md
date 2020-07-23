#基于Thymeleaf模板引擎

## 导入Jar包支持
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
## 配置路径`application.properties`
```properties
#thymeleaf 配置
#严格使用HTML5标准
spring.thymeleaf.mode=HTML5
#非严格使用HTML标准【强烈建议使用此标准】
#spring.thymeleaf.mode=LEGACYHTML5
spring.thymeleaf.encoding=UTF-8
spring.thymeleaf.content-type=text/html
#缓存设置为false, 这样修改之后马上生效，便于调试
spring.thymeleaf.cache=false
```

## 文件头部引入
```html
<!DOCTYPE html>
<!--必须必须引入-->
<html xmlns:th="http://www.thymeleaf.org">
<head></head>
<body></body>
</html>
```

<p style="text-indent:20px;">【说明】如果使用非严格HTML5标准，需要加入如下jar包</p>

```xml
<!-- thymeleaf legacyhtml5 模式支持 -->
<dependency>
    <groupId>net.sourceforge.nekohtml</groupId>
    <artifactId>nekohtml</artifactId>
    <version>1.9.22</version>
</dependency>
```
## 变量解析&字符串拼接
- 变量解析
```html
<p th:text="${name}">默认值</p>
```
- 字符串拼接
```html
<p th:text="|Hello! ${name}|">拼接方式一</p>
<p th:text="'Hello!'+${name}+'!!'">拼接方式二</p>
```
## 静态资源文件引入&url地址解析
```html
<!--样式引入-->
<link rel="stylesheet" type="text/css" media="all" th:href="@{/static/css/style.css}" />
<!--脚本引入-->
<script type="text/javascript" th:src="@{/static/js/base.js}"></script> 
<!--URL连接【不带参数】-->
<a th:herf="@{/lists}">跳转</a>
<!--URL连接【带参数解析（？号的形式）（/lists?param1=id1&param2=id2）】-->
<a th:herf="@{/lists(param1=${id1},param2=${id2)}">跳转</a>
<!--URL连接【带参数解析（以路径参数的形式解析）(/lists/id1)】-->
<a th:herf="@{/lists/{param1}(param1=${id1})}">跳转</a>
```
## 变量表达式
- 普通变量表达式(参与运算)：`<p th:text="${price+99}">默认值</p>`
- 类对象变量&普通变量表达式
```html
<p>直接只用对象域访问</p>
<!--user类对象-->
<div th:object="${user}">
    <p th:text="*{name}">user对象的属性值</p>
</div>
<p>直接使用对象名.属性访问</p>
<p th:text="${user.name}"></p>
<p>直接使用对象名.成员方法访问</p>
<p th:text="${user.getName()}"></p>
```
- javascript脚本中引入变量【内联】
```html
<script th:inline="javascript">
    var pid=[[$pid]];
</script>
```
- style中内联解析变量（暂无）【官方推荐的不管用，不识别`th:inline="css"`】(坑)
- 官方地址：[https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#inlining-vs-natural-templates](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#inlining-vs-natural-templates)
## 文件包含
```html
#include.html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
    <footer th:fragment="footer1">
        <span>Footer1</span>
        <p> Alll Rights Record</p>
    </footer>
    <footer th:fragment="footer2(start,now)">
        <span>Footer2</span>
        <p th:text="|${start}-${now} All Right|"></p>
    </footer>
</html>
#test.html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
    <div>
        <span>Replace</span>
        <!--不带参数写法--->
        <div th:replace="include::footer1"></div>
        <!---带参数写法-->
        <div th:replace="include::footer2(2018,2020)"></div>
    </div>
</html>
```
## if逻辑判断
- if判断
```html
<div>
    <p th:if="${testBoolean}">testBoolean为true时，显示此内容</p>
    <p th:if="${not testBoolean}">testBoolean为false时，显示此内容</p>
</div>
```
+ 真假判断规则说明
1. `boolean` 类型值为 `true` 时，返回 `true`，即显示；
2. 数值类型并且值不为0时，返回 `true`，即显示；
3. 字符类型（`Char`）并且值不是 `0`，返回true，即显示;
4. `String`类型并且值不是 `false`,`off`,`no`，返回 `true`,即显示;
5. 不是`boolean`，数值，字符，`String` 的其他类型，返回 `true`，即显示；
6. `null` 值，返回 `false`, 即不显示。

## 遍历变量
- 循环变量
```html
<tr th:each="p:${lists}">
    <td th:text="${p.name}"></td>
</tr>
<!--
把状态放在status里面
    1.index属性，从0开始索引
    2.count属性，从1开始索引
    3.sizes属性，集合内元素的总量
    4.current属性，当前的迭代对象
    5.even/odd属性，boolean类型，用来判定是否是偶数个还是奇数个，可用三元表达式
    6.first属性，boolean类型，是否是第一个
    7.last属性，boolean类型，是否是最后一个
-->
<tr th:each="p,status:${lists}">
    <td th:text="${p.name}"></td>
</tr>
```
- 选择遍历
```html
<!--size属性:多项显示-->
<select size="3">
    <!--th:selected 选中属性判断,th:value：属性的值-->
    <option th:each="p:${list}" th:value="${p.name}" th:selected="${p.name==currentValue}">
    </option>
</select>
```
- 单选遍历
```html
<!--th:checked选中属性判断，th:text文本属性值-->
<input type="radio" th:each="p:${lists}" th:value="${p.name}" th:checked="true" th:text="${p.name}" />
```
## 内置工具
1. Execution Info
2. Messages
3. URIs/URLs
4. Conversions
5. Dates
6. Calendars
7. Numbers
8. Strings
9. Objects
10. Booleans
11. Arrays
12. Lists
13. Sets
14. Maps
15. Aggregates
16. IDs

## 参考文档
- 官方文档：[https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#booleans](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#booleans)
- 参考文献一：[https://how2j.cn/k/springboot/springboot-thymeleat/1735.html](https://how2j.cn/k/springboot/springboot-thymeleat/1735.html)
- 参考文献二：[https://www.jianshu.com/p/908b48b10702](https://www.jianshu.com/p/908b48b10702)
- 参考文献三：[https://www.jianshu.com/p/964370d9374e](https://www.jianshu.com/p/964370d9374e)