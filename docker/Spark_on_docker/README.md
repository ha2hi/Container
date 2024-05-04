### Spark On Docker
----
Docker에 Spark를 설치하여 Saprk 작업 실행해보기
Docker와 Spark를 동시에 배우기 위함

### Spark를 Docker로 설치하면 장점이 뭘까?
1. 속도
Spark를 설치하기 위해서는 JAVA, HADOOP와 같이 앞에 설치 해야되는 부분이 있다. 
도커로 배포하면 빠르게 환경 세팅이 가능하다. 
2. 독립성
일관된 환경에서 Spark 작업을 실행 할 수 있다. 각 컨테이너를 구성하여 서로 영향을 주지 않을 수 있다.

### 설치 방법
- master 노드 1개와 worker 노드 2개로 구성하려고 한다.
- bitnami에서 제공하는 인증된 spark(버전:3.5.0) 이미지를 사용하려고 한다.
- 스파크 마스터 노드는 마스터 포트인 7077과 웹 UI를 위한 8080포트가 사용된다.
1. docker-compse 구성 및 배포
```
docker-compose up -d
```
2. Pyspark 파일 복사
실행중인 스파크가 정상적으로 작동하는지 테스트하려고한다.
간단한 pyspark 스크립트를 Master Node에 저장해야된다.
```
docker cp -L conn_spark.py my_spark_spark-master_1:/opt/bitnami/spark/anyfilename.py
```
3. spark 실행
docker exec my_spark_spark-master_1 spark-submit --master spark://172.13.5.185:7077 anyfilename.py

