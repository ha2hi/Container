# nginx 웹서버 설정 이해2

### default 파일의 server 설정

### listen

- listen은 HTTP 요청을 받을 포트 설정
- defalt_server는 모든 웹서버 요청을 받는다는 의미
- 두 번째 줄의 listen은 IPv6포트 관련 설정이므로 무시해도 됨

```docker
listen 80 default_server;  #IPv4 포트 관련
listen [::]:80 default_server; #IPv6 포트 관련

listen 8080 default_server;
listen [::]:80 default_server;
```

```docker
service nginx restart
```

### root

- 웹서버가 바라보는 root 폴더 위치
- nginx는 /var/www/html 경로가 default임

### server_name

- server_name은 요청 받을 도메인 이름 설정
- _는 도메인을 따로 설정 안하겠다는 의미

```docker
server_name naver.com www.naver.com;
```

### location

- location은 요청한 도메인 별로 분리를 하기위해 사용
- 예를 들어 [localhost:8080](http://localhost:8080)로 접속시 var/www/html/index.html파일을 return하고, [localhost](http://localhost:8080)/blog:8080으로 접속시 var/www/blog/index.html파일을 return해라와 같은 설정
- location 설정
    - location기본 설정 이해
        - 웹서버 주소에 따라 요청되는 파일을 찾아 디폴트 폴더를 root로 설정하고
        - location설정으로 웹서버 주소에 따른 폴더를 변경할 수 있음

```docker
    		location /blog {
                root /var/www; # localhost/blog/index.html
        }

        location /flask {
                root /var/www;
        }

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }

```

```docker
cd /var/www/html

cp index.nginx-debian.html ../blog/index.html

cp index.nginx-debian.html ../flask/index.html

service nginx restart
```

웹 브라우저에서 [localhost:8080/blog와](http://localhost:8080/blog와) [localhost:8080/flask](http://localhost:8080/flask) 접속시 정상적으로 접속되는지 확인하면 된다.

### 에러 해결

location을 지정해도 nginx를 찾을 수 없다고 나올 수 있다. 

그러면 설정 파일에서 유저 변경이 필요

```docker
vi /etc/nginx/nginx.conf

user root;
```