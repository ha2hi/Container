# docker 주요 명령 익히기(이미지를 다루는 다양한 옵션)

# Docker image 주요 명령

- 큰 그림으로 결국 다음과 같은 단계를 거치므로, docker image 관련 명령부터 알아보기로 함
    1. docker 설치
    2. docker image 다운로드
    3. 다운로드 받은 image로 docker container 생성 및  실행
    
- docker 실행 상태 확인

```docker
sudo systemctl status docker
```

- 모든 docker 명령은 CLI로 키보드로 직접 명령을 작성하는 형태로 수행하며, 명령 형식은 크게 다음과 같은 형태임

```docker
docker 명령 옵션 선택자(이미지ID/컨테이너등)
```

- docker는 image와 container 명령이 각각 별도로 존재

- 다음과 같이 image를 다루는지, container를 다루는지를 명시적으로 이해하기 위해, docker 다음에 image 또는 container를 기재해줌
    - 명령어는 어자피 다르므로, 굳이 image 또는 container를 붙이지 않아도 되지만, 최근에는 해당 키워드를 붙여주는 경향이 있음

```docker
docker image 명령 옵션 ...
docker container 명령 옵션 ...
```

# 1. 이미지 다운로드 받기위한 Docker Hub가입

- docker 이미지를 직접 작정해서 사용할 수 있음
- 파이썬/javascript 라이브러리처럼 손쉽게 미리 작성해놓은 docker 이미지를 다운로드받을 수 있음
    - 이를 위해 docker에서는 docker hub시스템을 제공함
- Docker Hub 가입 방법
    
    [https://hub.docker.com/](https://hub.docker.com/) 에서 가입   
    

Docker Hub 로그인 방법

```docker
docker login
```

[참고] docker hub에서 로그아웃도 할 수 있음

```docker
docker logout
```

# 2. 다운로드 받을 이미지 검색

```docker
docker search ubuntu
```

- docker 이미지는 크게 이미지명:태그로 이루어질 수 있음
- 태그는 보통 버전 정보를 넣는 경우가 많음
- 이미지명에 태그를 넣지 않으면, 최신 버전의 이미지를 의미하며, 최신 버전 이미지에는 가장 최신이라는 의미의 태그인 latest가 붙음
    - 따라서, 이미지명에 태그를 안붙이면 다운로드 받은 이미지의 태그명은 latest가 됨
    - 태그명을 안붙여도 됨에도, 기존 버전과 구별을 위해, 많은 가이드에서 이미지명:latest를 붙여서 다운로드를 하는 경우가 많음
- ubuntu로 검색하면 다양한 이미지 리스트를 볼 수 있음
    - 이 중에서 ubuntu라고 프로그램명만 있는 것이 공식 이미지라고 보면 됨
        - 더 확실한 방법은 리스트에 OFFICIAL 항목이[OK]라고 씌여 있으면 공식 이미지라고 보면 됨
        - nuagebec/ubuntu와 같이 /가 있는 경우는 / 앞단의 문자열이 Docker Hub의 사용자명이라고 보면 되고, 해당 사용자가 만든 이미지라고 보면 됨(즉, 공식 이미지가 아님)
- 너무 많은 이미지 검색 시, —limit옵션을 넣을 수 있음

```docker
docker search --limit=5 ubuntu
```

- DockerHub에 있는 특정 이미지의 태그 리스트를 볼 수 있는 공식적인 CLI는 없음
- 다음과 같이 dockerHub사이트 상에서 확인 가능함
    - [https://hub.docker.com/_/ubuntu?tab=tags&page=1&ordering=last_updated](https://hub.docker.com/_/ubuntu?tab=tags&page=1&ordering=last_updated)

# 3. 이미지 다운로드

- 태그를 안 붙이면, 최신 버전을 다운로드 받음

```docker
docker pull ubuntu
```

- 다음과 같이 특정 태그에 해당하는 버전을 다운로드받을 수도 있음

```docker
docker pull ubuntu:20.04
```

# 4. 다운로드 받은 이미지 목록 확인

- docker images 명령과 docker image ls 명령으로 동일한 기능을 수행할 수 있음

- docker images 명령

```docker
docker images

docker images -q # docker image id만 표시하기
```

- docker image ls 명령

```docker
docker image ls

docker image ls -q # docker image id만 표시하기
```