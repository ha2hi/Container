# docker 주요 명령 익히기1 (컨테이너를 다루는 다양한 옵션)

# 5. 다운로드 받은 이미지 삭제하기

- docker rmi 명령과 docker image rm 명령으로 동일한 기능을 수행할 수 있음

```docker
docker rmi 이미지ID(또는 이미지 REPOSITORY 이름)

dokcer image rm 이미지ID(또는 이미지 REPOSITORY 이름)

docker rmi -f 이미지ID # 강제로 이미지 삭제
```

# Docker Container 관련 주요 명령

# 1. 컨테이너 생성

- 각 이미지는 컨테이너로 만들어줘야, 실행 가능함
- 이미지와 컨테이너는 각각 관리해줘야 함
- 컨테이너 생성시, docker프로그램에서 이름이 자동 부여됨

```docker
docker create ubuntu
```

# 2. 생성된 컨테이너 확인

- 현재 실행 중인 컨테이너 확인

```docker
docker ps
```

- 실행 중이지 않은 컨테이너까지 포함해서, 전체 컨테이너 확인
    - 맨 끝의 NAMES에 기재된 이름이 컨테이너 이름임

```docker
docker ps -a
```

![Untitled](docker%20%E1%84%8C%E1%85%AE%E1%84%8B%E1%85%AD%20%E1%84%86%E1%85%A7%E1%86%BC%E1%84%85%E1%85%A7%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%86%A8%E1%84%92%E1%85%B5%E1%84%80%E1%85%B51%20(%E1%84%8F%E1%85%A5%E1%86%AB%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%82%E1%85%A5%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%83%E1%85%A1%E1%84%85%E1%85%AE%E1%84%82%E1%85%B3%E1%86%AB%20%20c1d92a2194a84f97a510725999e47f78/Untitled.png)

- 실행 중이지 않은 컨테이너 포함해서, 전체 컨테이너의 ID만 출력하기

```docker
docker ps -a -q
```

# 3. 컨테이너 삭제

```docker
docker rm 삭제할컨테이너이름
```

- 컨테이너 관리를 위해, 컨테이너 이름을 내가 원하는 이름으로 변경하는 방법
    - —name 옵션을 사용하면 됨

```docker
docker create --name 내가원하는컨테이너이름 이미지이름
```