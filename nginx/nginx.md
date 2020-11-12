### 静态文件提供下载

https://www.codexpedia.com/devops/nginx-serving-static-media-video-files/



nginx的静态文件路由配置的时候需要特别注意

```
    #location /sitemap/ {
    #   autoindex on;
    #   alias /home/liutaotao/work/happywrite/;
    #}
```

其中 路径 /home/liutaotao/work/happywrite/中包含 sitemap



### root vs alias



https://stackoverflow.com/questions/10631933/nginx-static-file-serving-confusion-with-root-alias



#### root

![image-20200428173818828](/Users/lttzzlll/Documents/notes/nginx.assets/image-20200428173818828.png)

![image-20200428174044080](/Users/lttzzlll/Documents/notes/nginx.assets/image-20200428174044080.png)



可以不以 / 结尾

####alias

![image-20200428173922857](/Users/lttzzlll/Documents/notes/nginx.assets/image-20200428173922857.png)

后缀必须以 / 结尾



### nginx 的正确配置姿势

```
server {
    server_name example.com;
    listen 80;
 
    index index.html;
    root /var/www/example.com;
 
    location / {
        #try_files $uri $uri/ =404;
        proxy_pass http://proxy;
    }
 
    location ^~ /images {
        alias /var/www/static;
        try_files $uri $uri/ =404;
    }
}
```



使用alias的场景：动态请求查看某些文件是否存在



一篇非常好的文章

https://www.techcoil.com/blog/understanding-the-difference-between-the-root-and-alias-directives-in-nginx/