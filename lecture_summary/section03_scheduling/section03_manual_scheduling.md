# Section 03 Manual Scheduling

Pod를 생성하게 되면 spec.nodeName은 공란으로 존재한다. 이를 쿠버네티스 스케줄러가 모든 Pod를 뒤지면서 NodeName이
공란이면 적절한 노드를 선택해서 노드에 바인딩한다.
- pending: 스케줄러가 pod를 노드에 할당하지 않은 상태

### 수동으로 Pod 스케줄링
~~~
apiVersion: v1
kind: Pod
metadata:
    name: mypod
spec:
    containers:
    - name: nginx
      image: nginx
    nodeName: worker-node-1
~~~
- 주의점으로는 Pod 생성 시에만 가능하기 때문에 후에는 nodeName 필드를 수정할 수 없다.

### 이미 생성된 Pod에 노드를 연결
- 이미 생성된 노드는 nodeName이 수정이 안되기 때문에 다음과 같은 방법을 사용한다.
- Binding 객체를 통해 POST 요청으로 스케줄링
~~~json
{
  "apiVersion": "v1",
  "kind": "Binding",
  "metadata": {
    "name": "mypod"
  },
  "target": {
    "apiVersion": "v1",
    "kind": "Node",
    "name": "worker-node-1"
  }
}
~~~
- `kubectl create -f binding.json --namespace=<namespace>`