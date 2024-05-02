### 에러 내용 정리
---
### 도커 관련
1. 도커 명령어 실행 에러(Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?)
- 도커 서비스가 실행하고 있지 않는 상태
- $sudo systemctl status docker (상태 확인)
- $sudo systemctl start docker (실행)
- $sudo systemctl enable docker (활성)