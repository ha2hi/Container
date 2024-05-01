# docker compose란?

# Docker Compose란?

- Docker Compose는 여러 컨테이너를 모아서 관리하기 위한 툴
- 웹 서비스는 프론트엔드 서버, 데이터베이스 서버, 백엔드 서버로 이루어져 있는 경우가 많음
    - 각각을 docker 컨테이너로 작성하고, 연결하여 동작하기 때문에 , Docker Compose와 같은 컨테이너 관리 툴이 필요함
- 더 나아가 서비스 규모가 커지면, 복수의 컨테이너를 유지하고 관리해야 하며, 이를 위해 쿠버네티스 등의 관리 툴이 사용됨

# Docker Compose 작성 기본

- Docker Compose는 docker-compose.yml 파일을 작성하여, 실행할 수 있음
- docker-compose.yml 파일은 YAML(야멜이라고 부름)형식으로 작성함