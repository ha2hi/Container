# 예제와 함께 익히는 docker compose 주요 명령2

```docker
vi docker-compose.yml

version: "3"
services:
	app:
		build:
			context: ./01_FLASK_DOCEKER
			dockerfile: Dockerfile
		links:
			- "db:mysqldb"
		ports:
			- "80:8080"
		container_name: appcontainer
		depends_on:
			- db
	
	db:
		images: mysql:5.7
		restart: always
		volumes:
			- ./mysqldata:/var/lib/mysql
		environments:
			- MYSQL_ROOT_PASSWORD=funcoding
			- MYSQL_DATABASE=fundb
		ports:
			- "3306:3306"
		container_name: dbcontainer
```

- 테스트 환경 셋업
    - [main.py](http://main.py) 작성
        - 제공한 01_FLASK_DOCKER폴더 내의 [main.py](http://main.py) 파일 사용
    - Docker Compose 실행
        - 크롬 브라우저를 통해, localhost:8080으로 접속이 가능하면 성공

```docker
cd 01_FLASK_DOCKER

vi Dockerfile

FROM continuumio/miniconda
COPY ./ /app
WORKDIR /app
RUN pip install flask pymysql cryptography

CMD ["python", "main.py"]
```

# dockerignore

- 위 flask Dockerfile작성식 COPY ./ /app 구문은 현재 폴더에 있는 모든 파일을 컨테이너 /app 폴더에 복사하는데 제외 파일을 지정할 수 있음
- ls -al로 숨김파일 목록을 확인해보면 파일 존재
- COPY시 특정 파일/폴더는 제외하도록 현재 폴더에 .dockerignore파일 작성 가능
- .dockerignore파일 포멧

```docker
# 주석
*/flask*
*/*/flask*
flask?
flask*
```