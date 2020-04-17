#### 简介
- Nginx-一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP代理服务器。
#### 功能
- 处理静态文件，索引文件以及自动索引；打开文件描述符缓冲
- 无缓存的反向代理加速，简单的负载均衡和容器
- 模块化结构，
- 支持SSL和TLS SNI。
- Master-slave模型，能够充分利用SMP的优势，且能减少工作进程在磁盘I/O的阻塞延迟。当采用select()/poll()调用时，还可以限制每个进程的连接数。
- 安装配置：[https://www.runoob.com/linux/nginx-install-setup.html](https://www.runoob.com/linux/nginx-install-setup.html)
- PHP安装配置：[https://www.cnblogs.com/kccdzz/p/9106513.html](https://www.cnblogs.com/kccdzz/p/9106513.html)
- 一键安装：[https://lnmp.org/](https://lnmp.org/)
- upstream参数制定。
- Nginx Rewrite 重写:[https://www.cnblogs.com/mikeluwen/p/7068279.html](https://www.cnblogs.com/mikeluwen/p/7068279.html)
```angular2html
    last #本条规则匹配完成后继续向下匹配新的location URI规则
    break #中止Rewirte,不在继续匹配
    redirect #返回临时重定向的HTTP状态302
    permanent #返回永久重定向的HTTP状态301 (会在浏览器中产生重定向缓存，-关闭服务器也可以访问（除非清除浏览器）)
    
    #重写示例：rewrite ^/(.*) http://www.bxvtest.com permanent;#永久重定向到http://www.bxvtest.com
    
    #正在匹配规则
    =       #表示精准匹配
    ^~      #表示uri 以某个字符串开头,理解为匹配url路径即可（以xx开头）。
    
    ~       #区分大小写的正则匹配（可用正则表达式）（以xx结尾）
    ~*      #不区分大小写的正则匹配（以xx结尾）
    
    !~和!~* #和上面两个恰巧相反
    
    / 通用匹配 任何请求都会
    
    首先精确匹配 = ->其次以xx开头匹配^~ -> 然后是按文件中顺序的正则匹配 -> 最后是交给 / 通用匹配。
    
    #文件及目录匹配
    *-f和!-f  #判断是否存在文件
    *-d和!-d  #判断是否存在目录
    *-e和!-e  #判断是否存在文件或目录
    *-x和!-x  #判断文件是否可执行
    
    #Nginx 的一些可用的全局变量，可用做条件判断
    $args
    $content_length
    $content_type
    $document_root
    $document_uri
    $host
    $http_referer #用来记录从那个页面链接访问过来的
    $http_user_agent  #用来记客户浏览器的相关信息
    $http_cookie
    $http_host #等于server_name
    $limit_rate
    $request_body_file
    $request_method
    $remote_addr  #用以记录客户端的IP地址
    $remote_port
    $remote_user #用来记录客户端用户名称
    $request_filename
    
    $request_uri
    $query_string
    $scheme
    $server_protocol
    $server_addr
    $server_name
    $server_port
    $uri
    
    #禁止访问某类型txt的文件,大括号前一定要有空格【仅指静态文件请求】
    location ~*\.(txt)$ {
        return 404;
    }
    
    #禁止某个目录访问【仅指静态文件请求】
    location ~ /invest {
           return 404;
    }
    
    #禁止某个目录下的文件访问：无法禁止php文件访问（原因未知）
    location ~ /invest/.*\.(txt)$ {
        deny all;
    }
        
     #禁止某个目录下的动态可执行文件（如PHP文件）访问时,匹配程度必须在【^~】以上
     location ^~/invest/ {
           return 404;
     }
     
     #通用规则可以用来转发后端服务请求[首页转发]
     location = / {
        proxy_pass http://www.baidu.com;
     }
     
     //如果Nginx日志太多，可以对一些文件进行过滤，然后临时关闭日志。
     location ~ .*\.(js|jpg|JPG|jpeg|JPEG|css|bmp|gif|GIF)$
     {
        access_log off;
     }
```
参考：[https://blog.csdn.net/qq_33862644/article/details/79337348](https://blog.csdn.net/qq_33862644/article/details/79337348)


###### Nginx Cache 服务配置
```angular2html
    #如果需要将文件缓存到本地，则需要增加如下几个子参数
    proxy_store on;
    proxy_store_access user:rw group:rw all:rw;
    proxy_temp_path 缓存目录;其中，
    proxy_store on 用来启用缓存到本地的功能，
    proxy_temp_path 用来指定缓存在哪个目录下，如： proxy_temp_path html;
    
    #虽然文件被缓存到了本地磁盘上，但每次请求仍会向远端拉取文件，为了避免去远
     端拉取文件，必须修改 proxy_pass：代码:
     
    if ( !-e $request_filename) {
        proxy_pass http://freeke;
    }
    
```

###### Nginx 负载均衡
- Nginx的upstrea目前支持4种方式的分配
    1. 轮询(默认)
       ：每个请求按时间顺序逐一分配到不同的后端服务器器，如果后端服务器down掉，能自动剔除。
    2. weight
       ：指定轮询几率，weight和访问比率程正比，用于后端服务器性能不均的情况
    3. ip_hash
       ：每个请求按访问的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。可以针对同一个C类地址段中的客户端选择同一个后端服务器，除非那个后端服务器宕机了才会换一个。
    4.fair(第三方)
       ：按后端服务器的响应时间来分配请求，响应时间短的优先分配。
    5. url_hash(第三方)
       ：按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。

```angular2html
    #每个设备的状态设置
    1.down 表示单前的 server 暂时不参与负载
    2.weight 默认为 1.weight 越大，负载的权重就越大。
    3.max_fails ：允许请求失败的次数默认为 1.当超过最大次数时，返回 proxy_next_upstream 模块定义的错误
    4.fail_timeout:max_fails 次失败后，暂停的时间。
    5.backup： 其它所有的非 backup 机器 down 或者忙的时候，请求 backup 机器。所以这台机器压力会最轻。
    Nginx 支持同时设置多组的负载均衡，用来给不用的 server 来使用。
    client_body_in_file_only 设置为 On 可以讲 client post 过来的数据记录到文件中用来做 debug
    client_body_temp_path 设置记录文件的目录 可以设置最多 3 层目录
    location 对 URL 进行匹配.可以进行重定向或者进行新的代理 负载均衡
```