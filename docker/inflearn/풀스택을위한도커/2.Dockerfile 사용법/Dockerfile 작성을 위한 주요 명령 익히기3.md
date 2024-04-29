# Dockerfile 작성을 위한 주요 명령 익히기3

### Kill

- docker stop은 즉시 컨테이너를 중단하지 않고, 현재 실행중인 단계까지는 기다린 후에 중지 함
- 이에 반해 docker kill은 즉시 컨테이너를 중지함

```docker
docker kill 컨테이너ID_또는_이름
```

### CMD 변경해보기

```docker
# Dockerfile_HTTPD 파일명으로 작성
FROM httpd:alpine

LABEL maintainer="dream@fun-coding.org"
LABEL version="1.0.0"
LABEL description="A test docker image to understand Docker"

COPY ./2021_DEV_HTML /usr/local/apache2/htdocs

CMD ["/bin/sh"]

docker build --tag myweb2 -f Dockerfile_HTTPD2 ./

docker run -dit -p 9999:80 --name httpdweb2 myweb2

```

- 이미지 조사하기
    - CMD가 변경된 것을 확인할 수 있음
    - 터미널에 접속할 수 있었던 것은, -dit 옵션으로 터미널로 연결된 채로, 컨테이너 실행시 /bin/sh 프로그램이 실해되면서, 대기 상태로 되었기 때문

- CMD 명령 덮어씌우기

```docker
docker run -dit -p 9999:80 --name httpdweb2 myweb2 /bin/sh -c httpd-foreground
```