### 특정 node에 Pod 배포하기(Node selector)
K8S의 노드 구조는 크게 Master/Worker/Infra/Ingress 노드로 구성됩니다.  
1. Master Node
- Worker 노드를 모니터링하여 Pod를 배포하고 K8S를 전체적으로 관리하는 노드입니다.
2. Worker Node
- 실제 Application을 배포하는 노드입니다.
3. Infra Node 
- Logging, Monitoring등이 설치되는 노드입니다.
4. Ingress Node 
- 부하분산을하기 위한 Ingress 설치되는 노드입니다. 
  
K8S 환경 구성시 목적에 맞춰 다양한 노드들로 구성합니다. 이 때 목적에 맞게 Pod를 지정한 노드에 배포해야 될 것 입니다.  
Pod를 원하는 노드에 배치시킬 수 있는 Label 기능 및 NodeSelector & Affinity에 대해서 알아보겠습니다.  

