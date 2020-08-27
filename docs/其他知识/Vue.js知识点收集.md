## 官方地址
- [https://cn.vuejs.org/v2/guide/computed.html](https://cn.vuejs.org/v2/guide/computed.html)

## 如何加载文件时自动预先加载自定义的方法
```javascript
new Vue({
    el:'#app',
    data:{
        message:'',
    },
    mounted:function() {
        //加载页面时，加载自定义函数
        this.getLists();
    },
    methods:{
        getLists:function() {
            //自定义函数
        }
    }   
})
```
## 基础组件的的使用方法？
- [https://cn.vuejs.org/v2/guide/components.html](https://cn.vuejs.org/v2/guide/components.html)

## 参考
- 参考JavaScript库【lodashjs】：[https://www.lodashjs.com/docs/lodash.chunk](https://www.lodashjs.com/docs/lodash.chunk)