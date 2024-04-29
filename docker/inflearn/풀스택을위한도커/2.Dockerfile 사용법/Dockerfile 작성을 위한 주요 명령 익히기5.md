# Dockerfile 작성을 위한 주요 명령 익히기5

### EXPOSE

- docker 컨테이너의 특정 포트를 외부에 오픈하는 설정
    - docker run -p 옵션으로 설정할 수도 있음
        - docker run -p 옵션은 컨테이너의 특정 포트를 외부에 오픈하고, 해당 포트를 호스트PC의 특정 포트와 매핑시킴
    - 이에 반해, EXPOSE는 컨테이너 생서 시, 특정 포트를 외부에 오픈하는 것만 설정하는 것임
        - 따라서 독립적으로 실행시는 EXPOSE 옵션을 넣는다고 해서, 호스트 PC에서 해당 컨테이너에 포트번호로 접속할 수 있는 것은 아님

```docker
# Dockerfile_HTTPD5 파일명으로 작성
FROM ubuntu:18.04
LABEL maintainer="a01045542949@gmail.com"

RUN apt-get update
RUN apt-get install -y apache2 apt-utils

EXPOSE 80

COPY ./2021_DEV_HTML /var/www/html/

ENTRYPOINT["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

```docker
docker build --tag myweb -f Dockerfile_HTTPD5 ./

docker run -P -d myweb # -P옵션을 쓰면, EXPOSE로 오픈된 포트에 호스트PC의 랜덤 포트가 매핑

```

### ENV

- 컨테이너 내의 환경변수 설정
- 설정한 환경변수는 RUN, CMD, ENTRYPOINT 명령에도 적용됨

```docker
# Dockerfile_MYSQL 파일명으로 작성
FROM mysql:latest

# mysql 슈퍼관리자인 root ID에 대한 password란에 원하는 패스워드 설정
ENV MYSQL_ROOT_PASSWORD=password

ENV MYSQL_DATABASE=dbname # dbname란에 원하는 데이터베이스 이름 설정

# 필요시 다음 설정도 가능
# ENV MYSQL_USER=user # user란에 mysql 추가 사용자 ID 설정
# ENV MYSQL_PASSWORD=pw # pw란에 mysql 추가 사용자 ID의 패스워드 설정
```

```docker
docker build --tag mysqldb -f Dockerfile_MYSQL

# docker 백그라운드 실행
docker run -d --name mydb mysqldb

# 컨테이너 접속해서 쉘 실행하기
docker exec -it mydb /bin/bash

# 이미지에서 설정한 패스워드 입력하기
mysql -u root -p

# mysql 내부에서 db가 있음을 확인할 수 있음
show databases;

exit

exit
```

### WORKDIR

- RUN, CMD, ENTRYPOINT 명령이 실행될 디렉토리 설정

```docker
FROM httpd:alpine

WORKDIR /usr/local/apache2/htdocs

CMD /bin/cat index.html
```

```docker
docker build --tag httpd6 ./

dodcker run -d --name myhttpd6 httpd6

docker logs myhttpd6
```

### 주석

주석은 “#”이다.

```python
# 주석 처리
```