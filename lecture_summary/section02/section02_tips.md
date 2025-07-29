# Section 02 Tips

- `--dry-run=client`: 기본적으로 명령어가 생성되면 바로 리소스가 생긴다. 이
옵션을 사용하면 test만 함.
- `o yaml`: 출력 결과를 yaml 형식으로 표현

### Pod 생성
- 바로 생성: `kubectl run nginx --image=nginx`
- yaml 템플릿 생성: `kubectl run nginx --image=nginx --dry-run=client -o yaml`

### Deployment 생성
- 바로 생성: `kubectl create deployment nginx --image=nginx --replicas=복제본 수`
- yaml 템플릿 생성: `kubectl create deployment nginx --image=nginx --dry-run=client -o yaml`
- yaml로 저장 후 수정
~~~
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml
수정명령어~
kubectl apply -f nginx-deployment.yaml
~~~

### Service 생성
- ClusterIP: `kubectl expose pod redis --port=6379 --name=redis-service --dry-run=client -o yaml
- 또는: `kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
`
  - 대신 label selector가 app=redis로 고정되어 수정이 필요하다,
- NodePort: `kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
`
- 또는: `kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
`
  - 마찬가지로 label selector 고정
  