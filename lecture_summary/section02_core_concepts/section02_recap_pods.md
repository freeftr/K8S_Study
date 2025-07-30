# Section 02 Recap Pods

scale-out 시 인스턴스를 늘릴 때에는 Pod를 새로 추가한다. 
- Pod는 애플리케이션과 보통 1대1 관계

그렇다고 Pod에 컨테이너가 한 개인가?
- 아니다 Pod에는 컨테이너가 여러 개 존재할 수 있지만, 그 컨테이너들이 같은 컨테이너가 아니다.

보통 주 애플리케이션 컨테이너와 나머지 보조 컨테이너로 구성을 한다.

## Pod 배포하기
***

### kubectl
~~~
kubectl run nginx
~~~
- Pod를 자동으로 생성하고, ngninx 컨테이너 배포
- 이미지는 Docker hub에서

~~~
kubectl get pods
~~~
- 현재 클러스터의 Pod 확인 가능

하지만, Service없이는 외부에서 접근 불가하다.