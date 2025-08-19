# Section 07 Image Security

### Image 네이밍 컨벤션
~~~
`[registry-domain]/[account or user]/[repository]:[tag]
~~~
- 기본값
  - registry 미지정 -> `docker.io`
  - user/account -> `library`

### Private Registry
- Kubelet이 이미지를 가져오는데 필요한 credential을 Secret에 저장해야 함.
1. Docker Registry Secret 생성
~~~
kubectl create secret docker-registry regcred \
  --docker-server=<registry-url> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-email=<email>
~~~
2. 생성된 Secret 확인
~~~
kubectl get secret regcred --output=yaml
~~~
3. Pod에 넣기
- `imagePullSecrets`에 추가
- 모든 pod가 해당 secret을 쓰기 원하면, 네임스페이스의 default ServiceAccount에 추가
~~~
kubectl patch serviceaccount default \
  -p '{"imagePullSecrets": [{"name": "regcred"}]}'
~~~

*공식문서 -> image pull -> Create Pod uses Secret 참조