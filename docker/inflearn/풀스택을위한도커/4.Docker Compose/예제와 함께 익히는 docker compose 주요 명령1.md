# 예제와 함께 익히는 docker compose 주요 명령1

Docker-compose를 사용하여 간단한 Flask 웹페이지와 DB를 배포해보려고 한다.

- 기존에 작성한 docker-compose.yml에 컨테이너를 추가하며, 추가 문법 이해하기

```docker
version: "3"

services:
  app:
    build:
      context: ./01_FLASK_DOCKER
			dockerfile: Dockerfile
		links:
			- "db:mysqldb"
		ports:
			-"80:8080"
		container_name: appcontainer
		depends_on:
			- db

	db:
		image: mysql:5.7
		restart: always
		volumes:
			- ./mysqldata:/var/lib/mysql
		environment:
			- MYSQL_ROOT_PASSWORD=funcoding
			- MYSQL_DATABASE=fundb
		ports:
			- "3306:3306"
		container_name: dbcontainer
```

### build

- 이미지를 Dockerfile을 기반으로 작성 시 사용
    - context: Dockerfile이 있는 디렉토리
    - dockerfile: Dockerfile명

### links

- 컨테이너 내부에서, 다른 컨테이너를 접속하고 싶을 때 사용
- 컨테이너명:사용할명

### container_name

- 사용하고 싶은 컨테이너 명
- container_name을 지정 안하고 배포시 docker-compose한 폴더명을 기반으로 컨테이너 명이 생성됨

### depends_on

- 해당 컨테이너를 생성 시 선행으로 생성되어야 하는 컨테이너 명을 지정
- 예를 들어, 현재 app 컨테이너는 db컨테이너가 먼저 생성되어야 연결하기 때문에 depends_on에 db 컨테이너 지정