# Section 02 Deployments

### Deployment
Deployment는 Pod, ReplicaSet 위에 오는 상위 개념의 오브젝트로 다음과 같은 일을 수행한다.
- 롤링 업데이트
- 롤백
- 일시 중지/재개

### Deployment 생성하기

1. definition 파일 생성
~~~
apiVersion: apps/v1
kind: Deployment
metadata:
    name: test-deployment
    labels:
        app: test
        type: front-end
spec:
    template:
        metadata: test-pod
        labels:
            app: test
            type: front-end
        spec:
            containers:
            - name: nginx-controller
              image: nginx
  replicas: 3
  selector:
      matchLabels:
          type: front-end
~~~

2. `kubectl create -f deployment-definition.yml` 실행