## JSP页面中获取获取完整的项目名
1. 方法一：直接获取 `${pageContext.request.contextPath}`
2. 方法二：JSP表达式获取 `<%=this.getServletContext().getContextPath()%>`

## 控制器方法中获取原生的API

```
@RequestMapping("testServlet")
public void testServlet(HttpServletRequest request, HttpServletResponse response){
  
}
```

