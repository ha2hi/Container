### 에러 내용 정리
---
1. 도커 명령어 실행 에러(Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?)
- 도커 서비스가 실행하고 있지 않는 상태
- $sudo systemctl status docker (상태 확인)
- $sudo systemctl start docker (실행)
- $sudo systemctl enable docker (활성)

2. nginx를 사용할 때 웹페이지를 찾을 수 없다고 나옴(location 오타x)
nginx.conf파일의 user를 변경 혹은 권한 추가
