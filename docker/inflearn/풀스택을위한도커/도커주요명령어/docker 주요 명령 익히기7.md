# docker 주요 명령 익히기7 (컨테이너를 다루는 다양한 옵션)

# 10. 실행중인 컨테이너에 명령 실행하기

- 컨테이너가 실행 중일 때만 다음 명령을 실행할 수 있음

```docker
docker exec 옵션 컨테이너_ID 명령 인자
```

- 테스트
    - -it : docker run에서 설명한 표준입력(-i), 터미널(-t) 옵션이며, docker exec에서도 사용 가능
    - 다음과 같이 명령하면 /bin/sh 쉘프로그램을 실행하면서 터미널에 입력되므로, 컨테이너 안으로 들어갈 수 있음
        - bin/bash 가 아닌 bin/sh를 쓴 이유는 /bin/bash는 alpine 리눅스에는 들어있지 않기 때문

```docker
docker exec -it apacheweb2 /bin/sh
```

# 11. 실행중인 컨테이너에 연결하기

- docker run으로 다음과 같이 터미널을 연결해놓은 상태로 백그라운드로 실행시
    - 여러 옵션은"-dit"와 같이 한 번에 붙여 써도 되고, 다음과 같이 나눠써도 됨

```docker
docker run -it -d --name myubuntu3 ubuntu

docker run -dit --name myubuntu3 ubuntu
```

- 다음과 같이 실행하면, 해당 컨테이너에 연결되어, 컨테이너 내에서 쉘 프로그램을 사용하여, 명령을 내릴 수 있음

```docker
docker attach myubuntu3
```

# 12. 모든 컨테이너 삭제하기(+모든 docker 이미지 삭제하기)

- 위와 같이 docker run 명령을 가장 많이 사용할 수 밖에 없는 상황이지만, docker run 사용 시 컨테이너가 별도로 생성 됨.
- 따라서, 모든 컨테이너를 한 번에 지우고 싶은 경우가 있으며, 다음과 같은 명령 조합으로 가능함
- 복, 붙 이용

```docker
# 모든 컨테이너 삭제를 위해, 우선 컨테이너를 모두 중지시킨 후, 삭제해야 함
docker stop $(docker ps -a -q) # 모든 컨테이너 중지

docker rm $(docker ps -a -q) # 모든 컨테이너 삭제
```

- 추가로 모든 docker 이미지 삭제 명령도 다음과 같음

```docker
docker rmi $(docker images -q)

docker rmi -f $(docker images -q)
```

- 별도로 다음 명령도 사용 가능
    - 하지만 다음 명령은 실행중인 container, 또는 실행 중인 image등은 삭제하지 않음
        - 따라서 생각보다는 많이 사용되지는 않음(참고)

```docker
docker container prune # 정지된 컨테이너 삭제

docker image prune # 실행중인 컨테이너 외의 이미지 삭제

docker system prune # 정지된 컨테이너, 실행중인 컨테이너 이미지 외의 이미지, 볼륨, 네트워크 삭제

```