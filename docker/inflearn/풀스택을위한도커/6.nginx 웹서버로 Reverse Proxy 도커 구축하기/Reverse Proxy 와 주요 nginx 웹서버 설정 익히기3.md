# Reverse Proxy 와 주요 nginx 웹서버 설정 익히기3

### nginx reverse proxy 테스트3 : 경로로 구분하기 (내부 서버에 요청하는 경로는 변경하기)

### 시나리오

- 예를 들어, 자신의 서버 /blog/index.html과 같이 proxy에 요청했을 때
- 내부 nginx에서는 자신의 서버 /index.html을 요청한 것처럼 경로 변경하기
- 기존에는 blog 폴더를 생성한 이후에 html을 만들었으나 폴더를 생성 안하고 배포할 수 있는 방법

```docker
proxy 요청 : 서버/blog/index.html
내부 nginx 요청 : 서버/index.html
```

- 이렇게 경로를 변경하기 위해 nginx rewrite 설정을 해야함
- nginx/nginx.conf 파일에서 docker-nginx 설정에서 rewrite 옵션을 추가

```docker
server {
	location /blog/ {
			rewrite         ^/blog(.*)$ $1 break;
			proxy_pass      http://docker-nginx;
...
```

- rewrite옵션 이해

```docker
rewrite regex URL [flag];
```

- regex : 매칭되는 URL 패턴을 설정
    - nginx에서 채택한 정규표현식 문법은 PCRE구문으로 본래 Perl이라는 언어에서 유래된 것임
- URL : 변경할 URL 기재
- flag : 여러 개의 location이 설정되어 있을 때 변경된 URL이 다른 location에 매칭될 수 있으므로 이를 어떻게 처리할지를 설정
    - break를 쓰면 변경된 url이 다시 다른 location 설정에 따르지 않고 현재의 location 설정만 따르고 끝냄
- 다음과 같이 작성하면
    - 다음 location 설정이 적용되는 original 경로는 /blog/ 를 포함하고 있음
    - ^ : 문자열 시작을 나타냄
    - 점(.) : 점은 임의의 한 문자를 의미
    - * : 0회 이상 나타나는 문자를 의미
    - $ : 끝나는 문자열을 의미
- 즉 ^/blog(.*)$는 경로가 /blog로 시작하면서 그 이후에 어떤 경로든 쭉 끝까지를 의미함
- $1 은(.*) 괄호로 되어 있는 부분을 의미함

```docker
server {
	location /blog/ {
			rewrite ^/blog(.*)$ $1 break;
```

- 따라서 위와 같이 작성하면
    - 경로에서 /blog를 뺀 다른 경로만을 [http://docker-nginx에](http://docker-nginx에) 전달하라는 의미임

### 참고(에러 페이지 설정)

```docker
error_page 403 404 405 406 411 497 500 501 502 503
504 505 /error.html;
	location = /error.html {
			root /usr/share/nginx/html;
	}
```

```docker
rewrite regex URL [flag];
```