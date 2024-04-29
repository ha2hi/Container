# Dockerfile 작성을 위한 주요 명령 익히기2

# LABEL

- 주석이라고 보면 됨. 이미지에 직접적인 영향 x
- LABEL은 <key> = <value> 형식으로 메타 데이터를 넣을 수 있는 기능
- 보통 저자, 버전, 설명, 작성일자등을 각각 key이름을 정하고, 값을 넣는 경우가 있음

```docker
FROM httpd:alpine
LABEL maintainer="hyucksangcho"
LABEL version="1.0.0"
LABEL description="A test docker image to understand Docker"
```

# COPY

- Dockerfile생성
- 내 로컬 서버에 있는 파일을 배포할 docker 이미지에 복사할 파일 지정
- COPY <복사할 파일명> <복사할 파일 위치>
```docker
FROM httpd:alpine
LABEL maintainer="dream@fun-coding.org"
LABEL version="1.0.0"
LABEL description="A test docker image to understand Docker"

COPY ./2021_DEV_HTML /usr/local/apache2/htdocs
```

### Inspect

- 이미지 조사하기
    - 다음과 같이 httpd:alpine 이미지와 직접 Dockerfile로 작성한 이미지 조사해보기

```docker
docker inspect myweb
```

# CMD

- 다음 세가지 형태로 CMD 명령을 작성할 수 있음
    - 명령어, 인자를 리스트처럼 작성하는 형태
        - 다음 예에서 echo 명령만 써줄 경우, 쉘에서 실행하지 않고, 직접 해당 명령이 실행되므로, 쉘에서 실행할 때 적용되는 쉘의 환경변수등 적용되지 않음(따라서 가장 정확하게는 명확히 쉘까지 지정해서 명령을 실행해주는 것이 좋음)
        - /bin/sh -c 명령은 /bin 디렉토리에 있는 sh프로그램을 실행하되,
            - -c옵션은 쉘명령을 터미널상에서 받지 않고, 인자로 받겠다라는 의미임
            - 따라서 /bin/sh -c echo라고 하면, echo라는 명령을 sh프로그램 상에서 실행하겠다라는 의미
            - 쌍따옴표로 써여 함
    
    ```docker
    CMD ["exectutable", "param1", "param2", ...]
    
    CMD ["/bin/sh", "-c", "echo", "Hello"]
    ```
    
    - ENTRYPOINT 명령어에 인자를 리스트처럼 작성하여 넘겨주는 형태
    
    ```docker
    CMD ["param1", "param2", ...]
    ```
    
    - 쉘 명령처럼 작성하는 형태
    
    ```docker
    CMD <command> <param1> <param2> ...
    ```
    
    > CMD는 하나의 Dockerfile에서 한 가지만 설정되며, 만약 CMD 설정이 여러개일 경우, 맨 마지막에 설정된 CMD 설정만 적용됨
    > 
    
    - httpd:alpine 기반 Dockerfile 작성하기
    
    ```docker
    # Dockerfile_HTTPD 파일명으로 작성
    FROM httpd:alpine
    
    LABEL maintainer="dream@fun-coding.org"
    LABEL version="1.0.0"
    LABEL description="A test docker image to understand Docker"
    
    COPY ./2021_DEV_HTML /usr/local/apache2/htdocs
    
    CMD ["/bin/sh", "-c", "httpd-foreground"]
    
    docker build --tag myweb2 -f Dockerfile_HTTPD ./
    
    docker run -d -p 9999:80 --name httpdweb1 --rm myweb2
    
    ```