# docker compose 포맷 이해

# Docker Compose 사용법 1

- Docker Compose 명령은 기본적으로 Dockerfile에서 익힌 명령에 기반하고 있음

> docker-compose.yml 예제 코드를 작성해보며 각 명령에 대해 이해해보기로 함
> 

```docker
version: "3"
services:
	db:
		image: mysql:5.7
		restart: always
		volumes:
		 - ./mysqldata:/var/lib/mysql
		environment:
			- MYSQL_ROOT_PASSWORD="12345678"
			- MYSQL_DATABASE="garnett"
		ports:
			- "3306:3306"
```

- 기본적으로 다음과 같은 4가지의 큰 카테고리로 작성하며, 이 중에서 보통 version과 services만 설정하여 사용함

```docker
# Docker Compose 파일 포맷 버전 지정
version "3"

# 컨테이너 설정
services:

# 컨테이너에서 사용하는 volume 설정으로 대체 가능(옵션)
volumes:

# 컨테이너간 네트워크 분리를 위한 추가 설정 부분(옵션)
networks:
```

### version

- Docker Compose파일 포맷 버전 지정
- docker 버전에 따라 지원하는 Docker Compose 버전이 있으며 기본적으로 버전 3으로 사용하는 것이 일반적임
- [https://docs.docker.com/compose/compose-file/compose-versioning/](https://docs.docker.com/compose/compose-file/compose-versioning/)

```docker
version: "3"
```

### services

- 위 항목 아래에서 여개 또는 하나의 컨테이너를 설정함
- 아래와 같은 경우에 db 컨테이너 한개를 생성하겠다라는 의미이다.

```docker
services:
  db:
    image: mysql:5.7
    restart: always
    volumes:
     - ./mysqldata:/var/lib/mysql
		environment:
			- MYSQL_ROOT_PASSWORD="12345678"
			- MYSQL_DATABASE="garnett"
		ports:
			- "3306:3306"
```

### Services - image

- 다음 코드에서 db는 컨테이너 이름을 정의한 것임
- db라는 이름의 컨테이너 작성시, Docker Hub에 있는 이미지를 사용할 경우, image를 설정하면 됨

```docker
services:
db:
	image: mysql:5.7
```

### Services - restart

- 컨테이너가 다운되었을 경우, 항상 재시작하라는 설정

```docker
services:
db:
	image: mysql:5.7
	restart: always
```

### Services - volumes

- docker run 옵션 중 -v 옵션과 동일한 역활
- 여러 volume을 지정할 수 있기 때문에 리스트 처럼 작성

```docker
services:
db:
	image: mysql:5.7
	restart: always
	volumes:
		- ./mysqldata:/var/lib/mysql
```

### Services - environment

- Dockerfile의 ENV 옵션과 동일한 역활

```docker
version: "3"

services:
	db:
		image: mysql:5.7
		restart: always
		volumes:
			- ./mysqldata:/var/lib/mysql
		environment
			- MYSQL_ROOT_PASSWORD=funcoding
			- MYSQL_DATABASE-fundb
```

- 다음 env_file 옵션으로 환경변수값이 들어가 있는 파일을 읽어들일 수도 있음
    - 패스워드등 보안이 필요한 부분을 docker compose보다는 별도 파일로 작성하여, env_file 옵션으로 읽어들이는 방식을 쓰는 경우도 많음

```docker
version: "3"

services:
	db:
		image: mysql:5.7
		restart: always
		volumes:
			- ./mysqldata:/var/lib/mysql
		env_file:
			- ./mysql.env
```

- mysql.env

```docker
cat mysql.env
MYSQL_ROOT_PASSWORD=funcoding
MYSQL_DATABASE=fundb
```

### Services - ports

- docker run의 -p 옵션과 동일한 역활
- YAML 파일 문법에서 숫자:숫자와 작성하면, 시간으로 해석하기 때문에 쌍따옴표를 붙여줘야 함

```docker
version "3"

services:
	db:
		images: mysql:5.7
		restart: always
		volumes:
			- ./mysqldata:/var/lib/mysql
		environment:
			- MYSQL_ROOT_PASSWORD=funcoding
			- MYSQL_DATABASE=fundb
		ports:
			- "3306:3306"
```