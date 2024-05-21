### eksctl example
https://github.com/eksctl-io/eksctl/tree/main/examples

### 노드 유형 확인 및 Pod 개수 제한 확인 명령어
```
kubectl get nodes -o jsonpath="{range .items[*]}{.metadata.labels['beta\.kubernetes\.io\/instance-type']}{'\t'}{.status.capacity.pods}{'\n'}{end}"
```