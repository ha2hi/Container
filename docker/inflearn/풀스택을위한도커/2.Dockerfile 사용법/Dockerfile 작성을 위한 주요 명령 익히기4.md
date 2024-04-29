# Dockerfile 작성을 위한 주요 명령 익히기4

# ENTRYPOINT

- ENTRYPOINT는 docker run시에 함께 넣어지는 CMD 명령에 덮어씌워지지 않고, 반드시 실행해야 하는 명령을 기입할 때 사용
    - 이때, docker run시 함께 넣어지는 명령은 ENTRYPOINT에 작성된 명령의 인자로 넣어지게 됨
    - 따라서, ENTRYPOINT에 컨테이너 실행시, 반드시 실행되야 하는 명령을 넣고, 별도로 각 컨테이너 생성시 필요한 인자는 docker run에 넣는  식으로도 활용하기도 함
- ENTRYPOINT로 변경해보기

```docker
# Dockerfile_HTTPD3 파일명으로 작성
FROM httpd:alpine
LABEL maintainer = "a01045542949@gmail.com"

COPY ./2021_DEV_HTML /usr/local/apache2/htdocs

ENTRYPOINT ["/bin/sh"]
```

```docker
# 이미지 생성
docker build --tag myweb3 -f Dockerfile_HTTPD3 ./

docker run -dit -p 9999:80 --name httpdweb3 myweb3 /bin/sh -c httpd-forefground

# 덮어 씌우기가 안될 것이다 왜 안될까?
docker inspect httpdweb3
```

- 위 에러를 조사하기 위해 다음과 같이 테스트

```docker
$ echo hi my name is hs # echo명령은 여러 인자를 그대로 화면에 출력
hi my name is hs
```

- 모든 이미지, 컨테이너 삭제

```docker
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker rmi -f $(docker images -q)
```

- 이미지 작성

```docker
# Dockerfile_HTTPD3 파일명으로 작성
FROM httpd:alpine
LABEL maintainer = "a01045542949@gmail.com"

COPY ./2021_DEV_HTML /usr/local/apache2/htdocs

ENTRYPOINT ["/bin/echo", "hello"]
```

```docker
# 이미지 생성
docker build --tag myweb3 -f Dockerfile_HTTPD3 ./

# 컨테이너 작성
dcoker run -dit -p 9999:80 --name httpdweb3 myweb3 /bin/sh hi

# 출력 확인
docker logs httpdweb3

hello /bin/sh hi
```

# RUN

- docker는 이미지 생성시 각 단계를 layer로 나누어 작성함
    - 이를 통해 특정 단계 변경시, 전체 이미지를 다시 다운로드 받지 않아도 됨
- RUN 명령은 이미지 생성시 일종의 layer를 만들 수 있는 명령으로, 보통 베이스 이미지에 패키지(프로그램)을 설치하여 새로운 이미지를 만들 때 많이 사용

```docker
# Dockerfile_HTTPD4 파일명으로 작성
FROM ubuntu:18.04
LABEL maintainer = "a01045542949@gmail.com"

RUN apt-get update
RUN apt-get install -y apache2 # apache2패키지 설치 중간에
Y/N 묻는 단계가 나오면 모두 YES로 하고 설치 

COPY ./2021_DEV_HTML /usr/local/apache2/htdocs

# apache2 웹서버 구동 명령은 다음과 같음
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

```docker
docker build --tag myweb4 -f Dockerfile_HTTPD4 ./

docker run -dit -p 9999:80 --name httpdweb4 --rm myweb4
```

- 다음과 같이 Dockerfile변경 후 이미지 작성시 일정 단계까지는 기존에 작성된 layer를 그대로 쓰는 것을 확인할 수 있음

```docker
# Dockerfile_HTTPD 파일명으로 작성
FROM ubuntu:18.04
LABEL maintainer = "a01045542949@gmail.com"

RUN apt-get update
RUN apt-get install -y apache2 apt-utils #apache2, apt-utils 설치
Y/N 묻는 단계가 나오면 모두 YES로 하고 설치 

COPY ./2021_DEV_HTML /usr/local/apache2/htdocs

# apache2 웹서버 구동 명령은 다음과 같음
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```