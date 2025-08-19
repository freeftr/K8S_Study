# Section 07 Service Accounts

- User Account: 사용자가 사용하는 계정
- Service Account: 애플리케이션이 쿠버네티스 클러스터와 통신하는데 사용하는 계정

### Service Account 생성
- `kubectl create serviceaccount dashboard-sa`
1. Service Account 객체를 먼저 만들고
2. 토큰을 Secret 객체에 넣는다.
   - Bearer 토큰으로 사용
   - v1.24부터는 Secret이 자동 생성되지 않는다.
   - `kubectl create token dashboard-sa`로 직접 발급
     - 1시간 유효

### Pod와 Service Account 연동
- Pod 생성 시 설정을 안하면, 자동으로 ServiceAccount의 토큰이 `/var/run/secrets/kubernetes.io/serviceaccount`에 마운트됨

