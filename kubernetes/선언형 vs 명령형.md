명령형 접근 방식은 CLI 명령을 통해 지시하는 방법이고, 선언형 접근 방식은 YAML 파일로 관리하는 방법입니다.  
더 자세히 알아보도록 하겠습니다.  

## 명령형
명령형 접근 방법은 CLI 명령으로 일일이 동작을 지시하는 방법입니다.  
"요구되는 환경을 어떻게(how)만들 것인가"에 초점을 두고 있습니다.  
예를 들어 k8s에서 다음과 같이 작업하는 방식을 명령형 접근 방법이라고 합니다.  
```
kubectl create deploy nginx --image=nginx:1.18.0

kubectl expose nginx --port=80

kubectl scale nginx --replicas=2
```
- 장점
1. 빠르고 간결한 작업 수행에 좋다
- 단점
1. 여러 파드를 구성과 같이 복잡도가 있는 경우 제한적이다.
2. 작업 추적이 어렵다.

## 선언형
선언형 접근 방법은 YAML 파일에 원하는 요소를 입력하여 CLI 명령을 통해 실행하는 방법입니다.  
"요구되는 환경이 무엇인가"에 초점을 두고 있습니다.  
예를 들어 k8s에서 다음과 같이 작업하는 방식을 선언형 접근 방법이라고 합니다.  
YAML 파일에 원하는 구성 요소를 입력 후 실행합니다.  
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        iamge: nginx:1.18.0
        ports:
          containerPort: 80
```
```
kubectl apply -f nginx.yaml
```
nginx라는 Deployment에서 replicas를 3으로 변경하고 싶다면 yaml 파일에서 replicas: 2 -> replicas: 3로 변경한 이후에 다시 nginx.yaml파일을 실행하면 됩니다.
- 장점
1. IaC 가능
2. 작업 추적 가능
- 단점
1. 간단한 작업인 경우 번거로움

## 명령형과 선언형을 혼용하면 안되는 이유
YAML 파일로 배포한 컨트롤러를 edit, scale같은 명령어를 사용하는 경우 의도하지 않은 결과가 나올 수 있다.  
활설 객체 설정(Live Object Configuration)이라는 개념과 연결되어 있다.  

### 활성 객체 설정(Live Object Configuration)
활성 객체 설정은 클러스터 안에에서 실행중인 객체에 대한 정보를 담고 있는 데이터입니다.  
  
kubectl apply를 실행하면 다음과 같은 과정을 따른다.  
1. YAML 파일 포맷의 원본 객체 구성 파일
2. 1번의 내용이 JSON 포맷으로 변환된 최신 적용 설정(last-applied-configuration)
3. 2번 내용을 어노테이션으로 포함하고 있는 활성 객체 설정
  
이제 kubectl apply 동작 방식을 나누어 보겠습니다.
1. 항목(필드)이 추가 혹은 수정
  1. 3번과 1번을 대조
  2. 만약 3번과 1번이 다른 경우 1번 기준으로 3번을 업데이트한다.
  3. 2번은 1번 내용으로 업데이트한다.
2. 항목(필드)를 삭제하는 경우
  1. 2번에는 있지만 1번에 없는 내용 확인
  2.  3번에서 제거
  3.  2번은 1번에 따라 업데이트

kubectl apply(선언형)을 실행하면 다음과 같은 과정을 거치는데 명령형(create, edit, scale) 작업을하면 2번을 업데이트하지 않고 3번만 수정하기 때문에 혼용해서 사용하면 내가 원하는 결과가 안나올 수 있는 것이다.  
