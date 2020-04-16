Ajax异步处理
- 下载Spring相关依赖包
```
jackson-databind.jar
jackson-core.jar
jackson-annotations.jar
```
1. 普通工程
链接：https://pan.baidu.com/s/1cX5rhtSzL6qy2LqUkBhCWA 
提取码：o1z9
2. maven工程地址
```xml
<!--【spring使用json的依赖包】-->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.7</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.9.7</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.7</version>
</dependency>
<!--【快速转换json的jar包】-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.68</version>
</dependency>
```
- jsp页面处理
```html
<a href="javascript:void(0);" id="btn">发送Ajax请求</a><br><br>
<script type="text/javascript">
        $(function(){
    	    $("#btn").click(function(){
    		$.ajax({
    		    url:"<%=this.getServletContext().getContextPath()%>/Index/testAjax",
    		    contentType:"application/json",
    		    data:'{"username":"java","birthday":"2019-03-01","sex":"男","address":"中国深圳"}',
    		    dataType:"json",
    		    type:"post",
    		    success:function(msg){
    		        alert(msg.username);
    			alert(msg.birthday);
    			alert(msg.sex);
    			alert(msg.address)
    		    }
    	        })
    	    })
    	})
</script>
```
- User对象
```
package com.bean;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

import java.io.Serializable;
import java.util.Date;
//JsonIgnoreProperties 用于Json数据封装对象时，忽略某个属性
@JsonIgnoreProperties(value = {"id","address"})
public class User implements Serializable {
    private int id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;
    ...........
}
```
- 控制器响应处理
```
//@RequestBody 用于请求到的json数据转换成User对象
//@ResponseBody 用于把返回的对象以json数据返回；
@RequestMapping(value="/testAjax")
public @ResponseBody User testAjax(@RequestBody User user) throws Exception{
    return user;
}
```
- 自定义快速转换JSON数据<br />
依赖包：`com.alibaba.fastjson.jar`

```
package com.tool;

import com.alibaba.fastjson.JSONObject;

public class JsonDataTransfer {
  /**
   * JSon数据发送
   * @param message 发送信息
   * @param retCode 发送状态码
   * @return JsonString
   *  
   */
    public String toJsonString(String message,String retCode) {
        if(message.equals("") || message.equals("")) {
            message="请输入发送信息";
        }
        if(retCode.equals("") || retCode.equals("")) {
            message="99";
        }
        JSONObject jsonObject=new JSONObject();
        jsonObject.put("message", message);
        jsonObject.put("retCode", retCode);
        String jsonString=jsonObject.toJSONString();
        //下面的方法，也可以
        //String jsonString=jsonObject.toString();
        return jsonString;
    }
    
    //object 发送的数据类型
    public String toJsonString(String message,String retCode,Object object) {
        if(message.equals("") || message.equals("")) {
            message="请输入发送信息";
        }
        if(retCode.equals("") || retCode.equals("")) {
            message="99";
        }
        JSONObject jsonObject=new JSONObject();
        jsonObject.put("message", message);
        jsonObject.put("retCode", retCode);
        jsonObject.put("data",object);
        String jsonString=jsonObject.toJSONString();
        //下面的方法，也可以
        //String jsonString=jsonObject.toString();
        return jsonString;
    }
}

```

**知识延伸**<br />
在配置springmvc.xml时遇到一个问题，不配置`<mvc:annotation-driven />`注解驱动时，控制器中的方法也可以访问；
但是当我们使用`<mvc:resources location="/js/" mapping="/js/**"/>`静态资源配置时，如果不配置注解驱动，控制器中的所有方法访问都会报404，如果配置，则可以正常访问,原因？
1. 因为当我们配置静态资源映射时，所有请求都去`<mvc:default-servlet-handler/>`即default servlet处理了;
2. 相应的控制器中的请求方法就无法匹配到了，即404;
3. 添加上“注解驱动”之后，相应的控制器会被注解驱动的自动注册的 `DefaultAnnotationHandlerMapping(处理器映射器)和AnnotationMethodHandlerAdapter(处理器适配器)`去自动处理，即去识别匹配controller层配置的路径，从而可以正常访问；




