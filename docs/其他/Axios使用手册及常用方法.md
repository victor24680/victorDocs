## 导入手册地址
- `<script src="https://unpkg.com/axios/dist/axios.min.js"></script>`
## 常用方式
```javascript
var url="getUser";//请求地址
var bean={ //请求数据【json对象】
        name:"张三",
        sex:'女'
    };
axios.put(url, bean).then(function (response) {
    //返回的数据【json对象】
    var jsonObject = response.data;
    //转换成json字符串
    var jsonString = JSON.stringify(jsonObject);
    //【控制台】浏览器控制台打印返回响应【默认是不会自动开启】
    console.log(response);
}).catch(function (error) {
    //【控制台】控制台打印错误信息【此对象为浏览器对象】
    console.log(error);
})
```
## 问题解析
问题：如何在ajax中使用普通参数请求？如：使用springMvc中的【@RequestParam】注解接收参数？<br />
1. 场景应用：使用SpringMvc中的注解@RequestParam注解接收参数时，只能从`key1=value1&key2=value2`，字符串中解析参数；
2. axios中Ajax（post）请求数据时，会自动把json对象数据【`{user:'11',sex:'11''}`】转换成json字符串数据，如果会导致404；
3. 解决方案如下；
4. 详细参考：[https://www.cnblogs.com/yiyi17/p/9409249.html](https://www.cnblogs.com/yiyi17/p/9409249.html)
```javascript
let param=new URLSearchParams();
param.append("user","11");
param.append("sex","11");
axios({
    method:'post',
    url:'/',
    data:param
})
```
## 参考
- 参考一：[https://www.jianshu.com/p/b239ae6a52ef](https://www.jianshu.com/p/b239ae6a52ef)
- 参考二(Web Api)：[https://developer.mozilla.org/zh-CN/docs/Web](https://developer.mozilla.org/zh-CN/docs/Web)