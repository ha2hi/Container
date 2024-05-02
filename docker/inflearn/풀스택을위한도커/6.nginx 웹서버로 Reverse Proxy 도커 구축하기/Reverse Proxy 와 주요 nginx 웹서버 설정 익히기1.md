# Reverse Proxy 와 주요 nginx 웹서버 설정 익히기1

### nginx reverse proxy 테스트1: 포트로 구분하기

- nginx reverse proxy 서버에 포트를 두 개 오픈한 후 각 포트 접속시, 각 내부 서버에서 결과를 가져오도록 구성
- proxy-server(nginx), nginx, apache 서버 구성

### nginx proxy 설정

```docker
mkdir nginx

cd nginx
vi nginx.conf

user nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile on;
    keepalive_timeout 65;

    upstream docker-nginx {
        server nginx:80;
    }

    upstream docker-apache {
        server apache:80;
    }

    server {
        listen 8080;

        location / {
            proxy_pass         http://docker-nginx;
            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
        }
    }
    server {
        listen 8081;

        location / {
            proxy_pass         http://docker-apache;
            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
        }
    }
```

### docker-compose.yml 파일 작성

```docker
version: "3"

services:
	nginxproxy:
		image: nginx:1.18.0
		ports:
			- "8080:8080"
			- "8081:8081"
		restarts: always
		volumes:
			- "./nginx/nginx.conf:/etc/nginx/nginx.conf"

	nginx:
		depend_on:
			- nginxproxy
		image: nginx:1.18.0
		restart: always
	
	apache:
		depend_on:
			- nginxproxy
		image: httpd:2.4.46
		restart: always
```

### docker-compse 실행 및 테스트

```docker
docker-compose up -d
```

웹 브라우저에서 8080,8081포트 접속 가능 여부 테스트