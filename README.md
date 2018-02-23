# nginx-cros-demo
nginx 跨域配置 demo



# 这里的 apis 是用来过滤你的请求那些需要被代理的
		#http://localhost:8080/apis/  会被 nginx 代理
		#http://localhost:8080/web/   不会被代理
		location /apis {
		
			add_header 'Access-Control-Allow-Headers' 'HEAD_INFO,Content-Type';
			add_header 'Access-Control-Allow-Origin' '*';
			add_header 'Access-Control-Allow-Credentials' 'true';
			add_header 'Access-Control-Allow-Methods' 'GET,POST,PUT';
			
			#rewrite  ^.+apis/?(.*)$ /$1 break;
			#include  uwsgi_params;
			
			proxy_pass    http://localhost:8080/apis/;  #代理到你后台 tomcat 的项目路径 /后面具体是哪个接口会自动转发请求的具体接口
            proxy_set_header Host 172.16.218.71;     
            proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header REMOTE-HOST $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			
		
        }
		#下面这里放你的 WEB 项目路径，里面有个 index.html
		root   C:/Users/xxx/Desktop/mediaReport168;