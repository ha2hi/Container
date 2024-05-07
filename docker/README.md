### 도커를 사용한 경험 내용 정리
1. 컨테이너가 실행되지 않는 경우
컨테이너를 실행해보는데 제대로 실행 안되는 경우 오타 때문에 발생 경험이 많은데 이 때 
`docker logs`로 로그를 확인하여 편하게 해결할 수 있었다.

2. 이미지와 디스크
AI 모델학습 결과를 AWS EKS(k8s 관리형 서비스)에 배포한 경험이 있다.  
당시에 image 사이즈가 커서 디스크 볼륨을 많이 차지했는데 몰라서 EBS 볼륨 사이즈를 낭비한 경험이 있었다.  
이미지 사이즈를 관리를 통해 볼륨 공간을 많이 차지할 수 있었는데 그 때 경험을 정리해 보겠다.    

[사용하지 않는 이미지 삭제]
`docker image prune` 혹은 `docker rmi` 명령어를 통해 사용하지 않는 이미지를 삭제한다.  

[캐시된 빌드 삭제]
`docker builder prune` 명렁어를 통해 이전 빌드에서 캐시가 사용될 가능성이 있으므로 빌드 캐시를 삭제합니다.  

[파이썬 이미지 사이즈 줄이는 방법]
1. Alpine 또는 Debian을 기본 이미지로 사용
2. 불필요한 패키지 제거
3. 패키지 버전 지정

- 참고
  - https://docs.docker.com/reference/cli/docker/image/prune/
  - https://docs.docker.com/reference/cli/docker/image/rm/
  - https://docs.docker.com/reference/cli/docker/builder/prune/
  - https://stackoverflow.com/questions/78105348/how-to-reduce-python-docker-image-size