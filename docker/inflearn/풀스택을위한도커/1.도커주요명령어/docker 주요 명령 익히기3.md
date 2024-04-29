# docker 주요 명령 익히기3 (컨테이너를 다루는 다양한 옵션)

# 5.  docker run 명령

ubuntu 자체만 컨테이너를 만들 경우, 다음 명령으로 터미널 및 입력(STDIN)을 연결해줘야 함

- ubuntu 컨테이너의 입력(STDIN) (-i 옵션)을 가상 터미널(-t 옵션)에 할당해주어, 결과적으로 PC 상에서의 입력이 ubuntu 컨테이너 입력에 들어갈 수 있도록 해줌
- 이를 통해 ubuntu 컨테의너의 bash 쉘은 입력 받을 수 있는 상태로, 종료되지 않고, 실행 중인 상태가 됨

### docker run 주요 옵션

![Untitled](docker%20%E1%84%8C%E1%85%AE%E1%84%8B%E1%85%AD%20%E1%84%86%E1%85%A7%E1%86%BC%E1%84%85%E1%85%A7%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%86%A8%E1%84%92%E1%85%B5%E1%84%80%E1%85%B53%20(%E1%84%8F%E1%85%A5%E1%86%AB%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%82%E1%85%A5%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%83%E1%85%A1%E1%84%85%E1%85%AE%E1%84%82%E1%85%B3%E1%86%AB%20%2053b0488419ee4e0a9a1f124b5ed9b9f2/Untitled.png)

### 참고 : -it 옵션의 의미

- docker 컨테이너에 표준 입력을 오픈해놓고(-i 옵션)
- pseudo tty를 만들어서 (-t 옵션) 해당 표준 입력을 pseudo tty에 연결해 놓음
- 따라서, 기보드 입력을 pseudo tty를 통해 컨테이너의 표준 입력으로 전달할 수 있도록 하는 것임

### 참고 : pseudo tty

- tty는 teletypewriter의 약자로, 리눅스(유닉스 계열)에서는 콘솔 또는 터미널을 의미함
- tty를 통해 리눅스에 키보드 입력을 전달할 수 있으며, 하나의 tty이외에 다양한 터미널에서 접속을 지원하기 위해, 두 번째 tty 부터 가상(pseudo)이라는 말이 붙어서, pseudo tty라고 이야기함

```docker
docker run -it ubuntu

docker run -it --name myubuntu ubuntu

docker ps -a
```

- 위와 같이 입력하면, ubuntu 내로 들어가서, 터미널로 명령을 내릴 수 있음
    - exit 명령하면, 도커 컨테이너에서 나올 수 있음
    - 해당 컨테이너는 중지됨
    
- 컨테이너 종료시 자동으로 컨테이너까지 삭제하는 옵션

```docker
docker run it --rm --name myubuntu2 ubuntu # exit 명령으로 종료시 컨테이너 자동 삭제됨

docker ps -a
```

- 컨테이너를 백그라운드로 실행하기(실행중인 상태이지만, 터미널로 입력은 받지 않은 상태로 만들어보기)

```docker
docker run -it -d --name myubuntu3 ubuntu

docker ps

docker attach myubuntu3
```