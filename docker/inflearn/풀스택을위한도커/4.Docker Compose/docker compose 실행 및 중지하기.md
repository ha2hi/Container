# docker compose 실행 및 중지하기

### Docker Compose 실행/중지 하기

- 다음 코드를 작업 폴더를 생성하여 docker-compose.yml 파일로 작성한다.

```docker
version: "3"

serives:
  db:
    image: mysql:5.7
    restart: always
    volumes:
		  - ./mysqldata:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD="12345678"
			- MYSQL_DATABASE="garnett_db"
    ports:
			- "3306:3306"
```

### Docker Compose 실행 명령

- 보통 -d 옵션을 사용하며, -d옵션은 백그라운드 실행을 의미함

```docker
docker-compose up -d
```

- 이미지 재빌드가 필요하면 —build 옵션을 추가해야함
    - 그렇지 않으면, 이미 작성된 이미지를 사용하게 됨

```docker
docker-compose up --build -d
```

### dokcer-compose 중지 명령

```docker
docker-compose stop
```

### docker Compose에서 사용하는 컨테이너 삭제 명령

- docker-compose up으로 생성된 컨테이너 삭제

```docker
docker-compose down
```