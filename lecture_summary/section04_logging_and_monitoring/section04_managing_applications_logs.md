# Section 04 Managing Application Logs

### 쿠버네티스 로깅
1. 기본 로그 확인 (첫 번째 컨테이너 확인)
- `kubectl logs <pod-name>`
- `kubectl logs -f <pod-name>`

2. 여러 개 컨테이너 존재 시 
`kubectl logs <pod-name> -c <container-name>`