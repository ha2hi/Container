# Dockerfile 작성을 위한 주요 명령 익히기1

# Dockerfile 이란?

- docker 이미지를 작성할 수 있는 기능임
- Dockerfile 문법으로 이미지 생성을 위한 스크립트를 작성할 수 있고, 이를 기반으로 이미지를 생성할 수 있음
- 나만의 이미지를 생성할 수 있고, 배포를 위해서도 많이 활용하는 기능임

# 1. Dockerfile 기본 문법

- Dockerfile은 텍스트 파일 형식이므로, 각자 사용하는 어떤 에디터로든 작성할 수 있음
- Dockerfile 기본 문법
    - 기본적으로는 간단히 명령과 인자로 이루어짐
    - 명령은 통상적으로는 대문자로 작성함(소문자로 작성해도 문제없지만, 명령임을 구별하기 위해 일반적으로 대문자로 작성함)

```docker
명령 인수
```

# Dockerfile 주요 명령

- Dockerfile 주요 명령에 대한 요약을 통해 큰 그림을 이해한 후 각 명령에 대해 상세히 이해하기로 함

[필수]

![Untitled](Dockerfile%20%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B1%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8C%E1%85%AE%E1%84%8B%E1%85%AD%20%E1%84%86%E1%85%A7%E1%86%BC%E1%84%85%E1%85%A7%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%86%A8%E1%84%92%E1%85%B5%E1%84%80%E1%85%B51%20eeef1837971649f2a0e656452e9ed0cc/Untitled.png)

[추가]

![Untitled](Dockerfile%20%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B1%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8C%E1%85%AE%E1%84%8B%E1%85%AD%20%E1%84%86%E1%85%A7%E1%86%BC%E1%84%85%E1%85%A7%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%86%A8%E1%84%92%E1%85%B5%E1%84%80%E1%85%B51%20eeef1837971649f2a0e656452e9ed0cc/Untitled%201.png)

- Dockerfile에서도 주석을 사용할 수 있음

```docker
# Dockerfile 주석은 # 을 쓰면, 해당 라인은 주석이 됨
```

# FROM

- 베이스 이미지 지정명령
- 반드시 Dockerfile에 작성해야 하는 명령

```docker
vi Dockerfile

FROM alpine
```

# Dockerfile로 이미지 작성

```docker
docker build 옵션 Dokerfile_경로
```

- 주요 옵션

![Untitled](Dockerfile%20%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B1%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8C%E1%85%AE%E1%84%8B%E1%85%AD%20%E1%84%86%E1%85%A7%E1%86%BC%E1%84%85%E1%85%A7%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%86%A8%E1%84%92%E1%85%B5%E1%84%80%E1%85%B51%20eeef1837971649f2a0e656452e9ed0cc/Untitled%202.png)

- 테스트
    - 위에서 작성한 Dockerfile이 있는 동일 경로에서 다음과 같이 명령
    - —tag test는 이미지 이름을 test로 설정한 것 이므로, 디폴트 태그가 붙어서 test:latest로 작성됨
    - 마지막의 "."은 현재 폴더를 나타내는 것이고, 즉 현재 폴더에 Dockerfile 파일이 있음을 의미함
        - 보다 명시적인 표기를 위해 "."대신 "./"와 같이 쓰는 것을 추천하기도 함

```docker
docker build --tag test .
```

- f 옵션 테스트

```docker
cp Dockerfile test_dockerfile
docker build --tag test2 -f test_dockerfile .
docker images
```

[실습]

### bulid

```docker
docker build --tag myimage ./

docker images

docker build --tag myimage:1.1 ./

docker images

mv Dockerfile Dockerfile2

docker build --tag myimage2 -f Dockerfile2 ./

docker images

```