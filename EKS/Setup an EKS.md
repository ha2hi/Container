### EKS 클러스터 생성
EKS 클러스터를 생성하기 앞서 EKS 클러스터를 관리하기 위한 인스턴스 및 노드그룹을 생성하기 위한 VPC와 서브넷을 구성합니다.  
그리고 EKS 클러스터를 관리하기 위한 EC2 인스턴스를 생성하고 AWS SDK, Docker, kubectl, eksctl 등 설치 및 IAM 자격증명(aws configure)이 필요합니다.  
마지막으로 EKS클러스터를 생성합니다.  

작업을 정리해 보자면 다음과 같습니다.
1. 기본 인프라 구성
2. EKS 클러스터 관리형 인스턴스 생성
3. EKS 클러스터 생성

1. 기본 인프라 구성
VPC 생성 및 서브네 생성(최소 2개의 가용영역)

2. EKS클러스터 관리형 인스턴스 생성
CLI 명령을 실행할 EC2 인스턴스 생성

3. EKS 클러스터 생성
```
eksctl create cluster \
  --name $CLUSTER_NAME \
  --region=$AWS_DEFAULT_REGION \
  --nodegroup-name=$CLUSTER_NAME-nodegroup \
  --node-type=t3.medium \
  --node-volume-size=30 \
  --vpc-public-subnets "$PubSubnet1,$PubSubnet2" \
  --version 1.26 \
  --ssh-access \
  --external-dns-access \
  --verbose 4
```

