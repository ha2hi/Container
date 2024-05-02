# Reverse Proxy 와 주요 nginx 웹서버 설정 익히기2

# nginx reverse proxy 테스트2 : 경로로 구분하기

> 위 설정을 기반으로 다음 부분만 수정
> 
- docker-compose.yml의 ports 부분만 다음과 같이 변경

```docker
version: "3"

services:
	nginxproxy:
		image: nginx:latest
		ports:
			- "80:80"
		restart: always
		volumnes:
			- "./nginx/nginx.conf:/etc/nginx/nginx.conf"
```

- nginx/nginx.conf 파일에서 docker-nginx 설정을 다음과 같이 변경
    - 이때 주소는 /blog / 부터 경로가 내부 nginx서버에 전송
        - 따라서 내부 nginx서버에 해당 경로에 지칭하는 파일이 있어야 함

![Untitled](Reverse%20Proxy%20%E1%84%8B%E1%85%AA%20%E1%84%8C%E1%85%AE%E1%84%8B%E1%85%AD%20nginx%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%86%A8%E1%84%92%E1%85%B5%E1%84%80%E1%85%B5%2062ae31ab088d4e31857e494e389394e4/Untitled.png)

```docker
docker ps

docker exec -it 아이디(엔진엑스) /bin/bash

cd /etc/nginx/conf.d

vi default.conf

cd /usr/share/nginx/html

mkdir blog

vi test.html
```

- location 위치와 폴더 위치는 같아야 된다.
- index.html은 알아서 찾으면됨