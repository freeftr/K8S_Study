# Section 03 Configuring Scheduler Profiles

## 스케줄링 흐름
***
1. Queuing
   - 대기열 정렬
   - PrioritySort 플러그인을 통해 PriorityClass 값에 따라 정렬
2. Filtering
   - 플러그인을 사용하여 리소스 부족 노드를 제외.
   - NodeResourcesFit
   - NodeName
   - NodeUnschedulable
3. Scoring
   - 남은 자원이 많은 노드에게 높은 점수 부여
   - ImageLocality
   - NodeResourceFit
4. Binding
   - 최종적으로 가장 높은 점수를 받은 노드에 Pod를 바인딩
   - DefaultBinder

## 스케줄링 Extension Point
***
각 단계는 Extension Point를 기반으로 동작하며, 이 곳에 Plugin을 삽입한다.
- 각 단계 앞뒤로 있음.
- QueueSort
- PreFilter
- Filter
- PostFilter
- PreScore
- Score
- Reserve
- Permit, PreBind
- Bind
- PostBind

## 동작 방식
***
### 기존 방식
기존에는 서로 다른 스케줄러 프로세스를 여러 개 돌리는 방식이었음
- 별도로 각각의 프로세스를 관리 비용 소모
- 경쟁 조건이 발생할 수 있다.

### Scheduler Profile
kube-scheduler의 설정 파일에 `profiles`를 추가해 여러 profile을 정의할 수 있다.
~~~
profiles:
  - schedulerName: default-scheduler
  - schedulerName: my-scheduler
    plugins:
      score:
        disabled:
          - name: NodeResourcesFit
        enabled:
          - name: MyCustomScoringPlugin
~~~