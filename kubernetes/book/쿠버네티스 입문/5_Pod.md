### Pod 개념
쿠버네티스는 파드(Pod)라는 단위로 컨테이너를 묶어서 관리합니다. 한 개의 Pod 안에는 한 개 이상의 컨테이너가 있습니다.  
Pod안에 속한 컨테이너들은 모두 노드 하나 안에서 실행됩니다. Pod안에 여러 개의 컨테이너가 있다고 하면 여러 개의 컨테이너가 여러 개의 노드에 분산되어 실행되지 않습니다.  

### Pod 템플릿
Pod의 기본적인 템플릿은 다음과 같습니다.
```
apiVersion: v1
kind: Pod
metadata:
  name: kubernetes-simple-pod             # 파드 이름
  labels:
    app: kubernetes-simple-pod            # 오브젝트를 식별하는 레이블
spec:
  containers:
  - name: kubernetes-simple-pod           # 컨테이너 이름
    image: garnett/simple-pod:latest      # 컨테이너에서 사용할 이미지 지정
    ports:
    - containerPort: 8080                 # 컨테이너에 접속할 포트번호 설정
```
```
# pod 싷행
kubectl apply -f pod-sample.yml
```

### 파드 생명 주기
파드는 생성부터 삭제까지의 생명 주기가 있습니다.
- pending
  - 파드가 생성중
- Running
  - 파드안에 모든 컨테이너가 실행 중인 상태
- Succeeded
  - 파드 안에 모든 컨테이너가 정상 실행 종료 상태
- Failed
  - 파드 안에 모든 컨테이너 중 정상적으로 실행 종료되지 않은 컨테이너가 있는 상태
- Unknown
  - 파드의 상태를 알 수 없는 상태
현재 파드의 생명 주기는 'kubectl describe pods 파드명`에서 Status 항목으로 알 수 있습니다.  

### kubelet으로 컨테이너 진단하기
컨테이너가 실행된 후에는 kubelet이 컨테이너를 진단합니다.  
이때 필요한 프로브(probe)는 다음 세가지가 있습니다.
1. livenessProbe
- 컨테이너가 실행 상태인지 확인
- 진단이 실패한 경우 컨테이너를 삭제 후 재실행
2. readlinessProbe
- 컨테이너가 실행 후 실제로 서비스 요청에 응답 가능한지 확인
3. startupProbe
- 컨테이너 안 애플리케이션이 실행되었는지 확인
- 진단이 실패한 경우 컨테이너를 종료 후 재실행

### 초기화(init) 컨테이너
초기화 컨테이너(init container)는 앱 컨테이너(메인 컨테이너)가 실행되기 전 파드를 초기화합니다. 보안상의 이유로 앱컨테이너 이미지와 같이 두면 안되는 앱의 소스 코드를 별도로 관리할 때 유용합니다.  
- 특징
  - 초기화 컨테이너가 여러개 있다면 파드 탬플릿에 명시한 순서대로 초기화 컨테이너가 실행됩니다.
  - 초기화 컨테이너가 실패하면 성공할 떄까지 재실행됩니다.
  - 초기화 컨테이너가 모두 실행된 이후에 앱 컨테이너가 실행됩니다.
  - kubectl get pods 명령어에서는 보이지 않습니다.
- 초기화 컨테이너 탬플릿
```
apiVersion: v1
kind: Pod
metadata:
  name: kubernetes-simple-pod
  labels:
    app: kubernetes-simple-pod
spec:
  initContainers:
  - name: init-myservice
    image: ubuntu
    command: ["sh", "-c", "sleep 2; echo helloworld01;"]
  - name: init-mydb
    image: ubuntu
    command: ["sh", "-c", "sleep 2; echo helloworld02;"]
```

### 파드 인프라 컨테이너
쿠버네티스의 모든 파드에는 항상 paues라는 컨테이너가 있습니다. 이 pause를 파드의 인프라 컨테이너라고 불립니다.  
Pause는 파드 안 기본 네트워크로 실행되며, 파드 안 다른 컨테이너는 pause 컨테이너가 제공하는 네트워크를 공유해서 사용합니다.  

파드 안에 있는 컨테이너를 재실행하는 경우 파드의 IP를 유지하지만, Paues 컨테이너를 재실행하는 경우 파드 안 모든 컨테이너도 재실행됩니다.  

pause가 아닌 다른 컨테이너로 파드의 인프라 컨테이너로 사용하는 경우 kubelet 명령 옵션으로 --pod-infra-container-iamge를 사용합니다.  

### 스태틱 파드
스택티 파드란 kube-apiserver를 통하지 않고 kubelet이 직접 실행하는 파드를 의미합니다.  
kubelet설정의 --pod-manifest-path라는 옵션에 지정한 디렉터리에 파드로 실행하려는 파드들을 넣어두면 kubelet이 감지하여 실행합니다.  
  
보통 스태틱 파드는 kube-apiserver 또는 etcd같은 시스템 파드를 실행하는 용도로 많이 사용됩니다.  
쿠버네티스에서 파드를 실행하려면 kube-apiserver가 필요한데 kube-apiserver자체를 처음 실행하는 수단으로 스태틱파드를 사용합니다.  

### 파드에 CPU와 메모리 자원 할당
쿠버네티스에서 하나의 노드 안에 자원 사용량이 많은 파드가 몰려있다면 파드들의 성능이 나빠질 것 입니다. 이럴 때 파드를 생성할 때 CPU나 메모리가 최소 얼마나 사용하고 최대 얼마나 사용할지 조건을 통해 배포하여 적절한 노드에 배포하게 설정할 수 있습니다.  
- 자원 사용량 지정 사용 가능 필드
  - limits.cpu
  - limits.memory
  - requests.cpu
  - requests.memory

- 리소스 지정 예시 탬플릿
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
  - app: my-pod
spec:
  Containers:
  - name: my-pod
    image: ubuntu
    resources:
      requests:
        cpu: 0.1
        memory: 150M
      limits:
        cpu: 0.5
        memory: 1G
    ports:
    - containerPort: 8080
```

### 파드에 환경 변수 설정하기
컨테이너를 사용할 때 장점 중 하나는 개발 환경에서 만든 컨테이너의 환경 변수만 변경해 실제 환경에서 실행하더라도 개발 환경에서 동작하던 대로 동작이된다.
- 파드에 환경변수 설정 탬플릿
```
apiVersion: v1
kind: Pod
metadata:
  name: kubernetes-simple-pod
  labels:
    app: kubernetes-simple-pod
spec:
  initContainers:
  - name: init-myservice
    image: ubuntu
    command: ["sh", "-c", "sleep 2; echo helloworld01;"]
  - name: init-mydb
    image: ubuntu
    command: ["sh", "-c", "sleep 2; echo helloworld02;"]
  containers:
  - name: kubernetes-simple-pod
    image: garnett/pod-image:latest
    resources:
      requests:
        cpu: 0.1
        memory: 200M
      limits:
        cpu: 0.5
        memory: 1G
    ports:
    - containerPort: 8080
    command: ["sh", "-c", "echo The app is Running! && sleep 3600"]
    env:
    - name: TESTENV01
      value: "testvaule01"
    - name: HOSTNAME
      valueFrom:
        fieldRef:
          fieldPath: spec.nodeName
    - name: POD_NAME
      valueFrom:
        fieldRef:
          fieldPath: metadata.name
    - name: POD_IP
      valueFrom:
        fieldRef:
          fieldPath: status.PodIP
    - name: CPU_REQUEST
      valueFrom:
        resourceFieldRef:
          containerName: kubernetes-simple-pod
          resource: requests.cpu
    - name: CPU_LIMIT
      valueFrom:
        resourceFieddRef:
          containerName: kubernetes-simple-pod
          resource: limits.cpu
```
- name : 환경 변수의 이름
- value : 문자열이나 숫자 형식의 값을 설정
- valueFrom : 어딘가에서 값을 참조
- fieldRef : 파드의 현재 설정 내용을 값으로 설정
- fieldPath : 어디에서 값을 가져올 것인지 지정
- resourceFieldRef : 컨테이너 CPU, Meomry 사용량에 대한 정보를 가져옴

### 파드 실행하기
YAML 형식으로 만든 Pod 선언 탬플릿을 실행하려면 다음과 같이 실행하면된다.  
'kubectl apply -f 탬플릿_파일명'

### 파드 구성 패턴
파드로 여러 개의 컨테이너를 묶어 실행할 때 몇가지 패턴이 있다.  
1. 사이드카 패턴
사이드카 패턴은 원래 사용하던 기본 컨테이너의 기능을 확장하기위한 컨테이너를 추가하는 것입니다.  

예를 들어, 웹서버 컨테이너가 있고 로그를 남기고 있다고 한다면 사이트카 역할을 하는 로그를 수집해 외부로 보내는 로그 수집 컨테이너를 생성합니다.  
이 때 웹서버 컨테이너를 삭제한다고 해도 로그 수집 및 전송 역할을 하는 컨테이너는 재활용하여 사용할 수 있습니다.  
즉, 공통 역할을 하는 컨테이너의 재사용성을 높일 수 있습니다.  

2. 앰배서더 패턴
앰배서더 패턴은 파드 안에 프록시 역할을하는 컨테이너를 추가하는 패턴입니다.  

예를 들어, 웹 서버 컨테이너는 캐시에 localhost로만 접근하고 실제 외부 캐시 중 어디로 접근할지는 프록시 컨테이너가 처리합니다.  
이런 방식으로 파드의 트래픽을 더 세밀하게 제어할 수 있습니다.  

3. 어댑터 패턴
어댑터 패턴은 파드 외부로 노출되는 정보를 표준화하는 어댑터 컨테이너를 사용한다는 뜻입니다.  
주로 어댑터 컨테이너로 파드의 모니터링 지표를 표준화한 형식으로 노출시키게 합니다.  
