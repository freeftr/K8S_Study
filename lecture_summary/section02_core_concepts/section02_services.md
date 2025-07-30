# Section 02 Services

Service는 애플리케이션 내부 또는 외부 구성 요소 간 통신을 가능하게 해준다.
- 외부에서 Pod내의 IP에 직접 접근 불가하다.

Service는 이 과정에서 지정된 포트에서 요청을 리스닝하다가, 해당 요청을
매칭되는 Pod로 포워딩해준다.

## Service의 종류
***
- NodePort
- ClusterIP
- LoadBalancer

## NodePort
***
NodePort는 3가지 포트로 구성된다.
- targetPort: Pod 내부에서 서비스가 리스닝 중인 포트
- port: 서비스 자체 포트
- nodePort: 외부에서 접근할 수 있는 노드의 포트
  - 30000~32767

### Service 생성
1. definition 생성
~~~
apiVersion: v1
kind: Service
metadata:
  name: test-service
spec:
  type: NodePort
  ports:
   - targetPort: 80
     port: 80
     nodePort: 30008
  selector:
     app: test-app
     type: front-end
~~~
- targetPort를 명시하지 않으면 port랑 같은 값 할당
- nodePort를 명시하지 않으면 범위 내 자동 할당
2. `kubectl create -f service-definition.yml`

### Pod가 여러개 있으면?
기본적으로는 동일한 Pod 모두 연결한다. Pod가 여러 노드에 분산되어 있어도 NodePort는
모든 노드에서 동일한 포트로 열린다.

## Services CLuster IP
***
하나의 웹 애플리케이션은 여러 파트로 나뉘어 다양한 Pod로 실행된다.

Pod는 IP 주소를 가지지만, 동적이기 때문에 이를 쓸수는 없다. Pod끼리 통신을 해야할 때 그럼 어떡하는가?

Service는 이때 동일한 역할을 하는 Pod를 그룹화하고 고정된 가상 IP를 제공한다. 그리고 그룹화된
Pods 중 하나로 로드밸런싱한다.

### Cluster IP 생성
1. definition 생성
~~~
apiVersion: v1
kind: Service
metadata:
  name: test
spec:
  type: ClusterIp
  ports:
   - targetPort: 80
     port: 80
  selector:
    app: test
    type: back-end
~~~
2. `kubectl create -f definition-cluster-ip.yaml`

## LoadBalancer
***
NodePort는 각 노드의 고유 포트를 통해 외부에서 접근하게 한다.
- 그럼 이제 IP:Port 조합을 사용해야 하는데 사용자 입장에서 불편한다.
- 단일 도메인 사용 불가능

두 가지 방법이 있다.
- 수동 방법
  - 각 노드의 NodePort로 라우팅 설정
  - 직접 VM 생성해서 수동으로 로드밸런서 구성
  - 단점: 복잡
- 클라우드 네이티브
  - 서비스 타입을 NodePort로 지정
  - 클라우드 플랫폼의 네이티브 로드밸런서가 자동 구성
  - DNS 및 IP 관리 자동화
  - 단점: 클라우드 환경에서만 동작