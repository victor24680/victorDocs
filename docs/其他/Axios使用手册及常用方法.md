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
## 参考
- 参考一：[https://www.jianshu.com/p/b239ae6a52ef](https://www.jianshu.com/p/b239ae6a52ef)