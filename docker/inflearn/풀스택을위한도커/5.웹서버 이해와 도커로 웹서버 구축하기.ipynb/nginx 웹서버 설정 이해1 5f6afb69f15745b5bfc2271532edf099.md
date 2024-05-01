# nginx 웹서버 설정 이해1

실습을 하기 이전에 nginx 서버 파일에 대한 기본적인 이해가 필요하다.

ubuntu : 20.04

nginx : latest

실습

### Nginx 기본 사용법

```docker
docker run -dit -p 80:8080 --name myos ubuntu:20.04
docker exec -it myos /bin/bash

apt-get update
apt-get install nginx=1.18.0-0ubuntu1# 6. Asia 69. Seoul 
# nginx 설치 에러 발생 apt-cache policy nginx

apt-get install vim
```

### nginx.conf

- nginx 웹 서버 기본 설정 파일
- 크게 user, worker_processes, pid, events, http 항목으로 이루어짐
- 이 중에서 http 블록이 전체 웹서버 기본 설정 항목임

```docker
find -name nginx.conf # 가볍게 위치만 확인
vi /etc/nginx/nginx.conf
```

### 참고

- nginx.conf 파일을 확인해보면 site-enabled 폴더가 활성화되어 있다는걸 알 수 있음. 그러나 site-enabled 폴더는 sites-available가 심볼링 링크로 설정되어 있음
- 즉 /etc/nginx/sites-available/ 폴더에 파일을 작성해놓으면 자동으로 etc/nginx/sites-enabled/폴더에 넣어진것으로 보이게 되고, 해당 파일들은 nginx.conf의 http 항목의 include 명령을 통해, 웹서버 설정에 적용됨
- 그렇기 때문에 /etc/nginx/sites-available/default 파일 설정 변경 후 nginx 재실행 시 수정이 반영이 되는 것임

- 다음 작업을 진행한 후, localhost:8080접속

```docker
vi sites-available/default

------------- default 파일 수정
server{
				listen 8080 deafalut_server; # listen 80을 8080으로 변경
-------------

service nginx restart
```