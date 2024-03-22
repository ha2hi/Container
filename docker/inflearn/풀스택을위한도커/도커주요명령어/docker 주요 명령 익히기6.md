# docker 주요 명령 익히기6 (컨테이너를 다루는 다양한 옵션)

- 참고로 이해하면 됨

# docker가 사용하고 있는 저장매체 현황 확인하기

- 추후 docker가 사용하는 저장매체 공간이 이슈가 될 수도 있으므로, 관련 명령어를 익혀둬야 함

```docker
docker system df

# df  전체 공간 확인
```

## docker와 alpine

- docker 이미지는 여러개의 이미지가 계층으로 쌓인 형태로 작성이 됨
    - 예를 들면, C라이브러리 이미지를 쌓고, 여기에 bash와 같은 쉘 프로그램 이미지를 쌓고, 여기에 응용프로그램 이미지를 쌓는 방식
    - 통상 리눅스 사용시, 다양한 기능을 가진 ubuntu등의 리눅스 패키지를 사용하지만, docker 컨테이너의 경우는 특정 응용 프로그램 실행을 목적으로 하는 경우가 많기 때문에, 다양한 기능을 모두 포함할 필요가 없음(동일한 기능을 한다면, 도커 이미지/컨테이너 사이즈가 작으면 작을 수록 좋음)

- 대부분의 docker 이미지에 가장 기본이 되는 이미지는 ubuntu가 아니라 alpine인 경우가 많음
    - alpine은
        - musllibc라는 임베디드 리눅스(초경량 시스템을 위한 리눅스 시스템)를 위한 C/POSIX library(C언어를 위한 기본 함수 및 POSIX라는 표준 규격에 맞춘 기본 함수를 포함한 라이브러리)외
        - BusyBox는 운영체제 운영에 필요한 가장 기본이 되는 유틸리티(시스템 프로그램)만 모아놓은 리눅스 패키지

### httpd와 alpine

- httpd도 태그 중에 alpine기반 태그가 있음
    - [https://hub.docker.com/_/httpd?tab=tags&page=1&ordering=last_updated](https://hub.docker.com/_/httpd?tab=tags&page=1&ordering=last_updated)
- httpd:alpine 실행해보기

```docker
docker run -d -p 80:80 -v /home/ubuntu/2021_DEV_HTML:/usr/local/apache2/htdocs --name
apache2 httpd:alpine
```

# 실행중인 컨테이너 사용 리소스 확인하기

- 실행 중인 컨테이너의 시스템 리소스 사용현황을 확인할 수 있음
- 종료는 Ctrl+c로 종료 할 수 있음

```docker
docker container stats
```