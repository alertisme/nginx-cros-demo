# nginx-cros-demo
## nginx 跨域问题解决方案二 （前端自己解决系列）
至于方案一（后端解决），不推荐，懒得写了不过我会给你一个关键词：CrossOriginFilter.java 剩下的自己解决吧。
（后来我又解决了为了避免别人骂，惊喜都在代码里面了，自己发掘吧）

之所以写这篇文章呢是因为很多你认为很简单的东西却有很多人不知道。
写之前搜了一下网上，发现没有一个是系统的讲这些的东西的，于是自己动手写了一个全套服务配置齐全的。

高手请直接看 nginx.conf 和 index.html 就 OK。
剩下的所有东西是为了给不了解不明白的新人搭建环境用的。



Mybatis-Spring-master.zip 这个是一个 JAVA WEB 项目，其实不是给你看的，是给你们后端开发人员看的，如果他说跨域问题解决行不通要么是他懒，要么是他笨，什么都别说，怼他！ 怼他！ 怼他！

nginx-1.13.4.zip 这个是 nginx 服务器，如果你不了解请去百度了解这个东西。

web.zip 这个是一个超级简单的静态前端项目，如果作为一个前端你看不懂，那么请点击右上角的那个 X 关闭此网页，再见。


# 接下来是重点了

---
1. 将 web.zip 解压放在你的 D 盘根目录下。
2. nginx.zip 解压放在哪里都可以，然后双击 nginx.exe 执行。
3. 解压 Mybatis-Spring-master.zip 并跑起来整个 java 项目，别说你是前端不会，不会的话请出门右拐下楼梯。
---

# 以上都是废话，其实这里才是所有的重点中的重点如果你懂 nginx 配置的话。
```
这里的 apis 是用来过滤你的请求那些需要被代理的
http://localhost:8080/apis/  会被 nginx 代理
http://localhost:8080/web/   不会被代理

location /apis {
	add_header 'Access-Control-Allow-Headers' 'HEAD_INFO,Content-Type';
	add_header 'Access-Control-Allow-Origin' '*';
	add_header 'Access-Control-Allow-Credentials' 'true';
	add_header 'Access-Control-Allow-Methods' 'GET,POST,PUT';
	
	proxy_pass    http://localhost:8080/apis/;  #代理到你后台 tomcat 的项目路径 /后面具体是哪个接口会自动转发请求的具体接口
	proxy_set_header Host 172.16.218.71;     
	proxy_set_header X-Real-IP $remote_addr;
	proxy_set_header REMOTE-HOST $remote_addr;
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	

}
下面这里放你的 WEB 项目路径，里面有个 index.html
root   D:/web;
```
